# sheet-to-json

Directly from Google Docs


```function sheetToJSON() {
  const rows = SpreadsheetApp.getActiveSheet().getDataRange().getValues();
  const skipRows=[0]
  const arrayColumns=[11,14,15,16]
  const timeColumns = [12,13]
  const finalFormat={}
  rows.map((row,idx)=>{
        const data={}
        if(skipRows.includes(idx)){
          return;
        }
        row.map((column,columnIdx)=>{
          if(arrayColumns.includes(columnIdx)){
            column=column?.split("\n")
          }
          if(timeColumns.includes(columnIdx) && typeof column === "object"){
           column=Utilities.formatDate(column, "UTC", "hh:mm a");
           const [time, modifier] = column.split(' ');
           let [hours, minutes] = time.split(':');
           if (hours === '12') {
              hours = '00';
           }
           
           if (modifier === 'PM') {
            hours = parseInt(hours, 10) + 12;
           }
           column= `${hours}:${minutes}`;
           console.log(column)
          }
          data[rows[0][columnIdx]]=column;
        })
        finalFormat[rows[idx][0]]=data;
      })
      console.log("FINAL DATA",JSON.stringify(finalFormat,null,"\t"))
}
