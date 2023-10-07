function waReminder() {

  const headers = {
    'Authorization': 'TOKEN',
    'Content-Type': 'application/json',
    'Accept': 'application/json'
  };

  var spreadSheet = SpreadsheetApp.openByUrl(SpreadsheetApp.getActiveSpreadsheet().getUrl());
  var sheet = spreadSheet.getSheets()[0];
  var rangeValues = sheet.getRange(2, 1, sheet.getLastRow() - 1, 5).getValues();

  for (var i in rangeValues) {
    var employeeName = sheet.getRange(2 + Number(i), 1).getValue()
    var activity = sheet.getRange(2 + Number(i), 2).getValue()
    var phoneNumber = sheet.getRange(2 + Number(i), 4).getValue()
    var place = sheet.getRange(2 + Number(i), 5).getValue()

    var todayDate = Utilities.formatDate(new Date(), Session.getScriptTimeZone(), "dd MMMM yyyy"); // 21 April 2023
    
    var trainingDate = new Date(sheet.getRange(2 + Number(i), 3).getValue());
    var formattedTrainingDate = Utilities.formatDate(trainingDate, Session.getScriptTimeZone(), "dd MMMM yyyy"); // 23 April 2023
    
    var reminderDate = new Date(trainingDate); // Change day reminder
    var formattedReminderDate = Utilities.formatDate(reminderDate, Session.getScriptTimeZone(), "dd MMMM yyyy"); // 21 April 2023

    Logger.log(todayDate);
    Logger.log(formattedReminderDate);
    var dataCompare = convertDate(todayDate, formattedReminderDate)

    const requestBody = {
      'target': String(phoneNumber),
      'message':
        '*_This is an auto generated message, please do not reply._*\r\n\r\n' +
        'Dear ' + employeeName + ',\r\n' +
        'Ini adalah pengingat tentang pelatihan Anda pada :\r\n\r\n' +
        'Tanggal : ' + formattedTrainingDate + '\r\n' +
        'Subject : ' + activity + '\r\n' +
        'Lokasi : ' + place + '\r\n\r\n' +
        'Mohon untuk datang tepat waktu dan berpakaian dengan Sopan.'
    };

    var bodyMessage = JSON.stringify(requestBody);
    var result = sheet.getRange(2 + Number(i), 6);
    var remark = sheet.getRange(2 + Number(i), 7);

    try {
      if (compareDates(dataCompare) == 0 && (result.isBlank() || result.getValue() === 'FAILED')) {
        var response = UrlFetchApp.fetch('https://api.fonnte.com/send',
          {
            method: 'POST',
            payload: bodyMessage,
            headers: headers,
            contentType: "application/json"
          });
        result.setValue('SUCCESSFUL').setBackground('#b7e1cd');
        remark.setValue('Sent on ' + new Date());

        Logger.log(response)
      }
    } catch (err) {
      result.setValue('FAILED').setBackground('#ea4335');
      remark.setValue(String(err).replace('\n', ''));
    }
  }

}

function convertDate(date1, date2){
  
  var months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
  var arrayDate1 = [];
  var arrayDate2 = [];
  arrayDate1 = date1.split(" ");
  arrayDate2 = date2.split(" ");
  var intDate1 = [];
  var intDate2 = [];
  var month1;
  var month2;

  //month parse
  for(var i=0; i<12; i++){
    if(months[i] === arrayDate1[1]){
      month1 = i + 1;
    }
  }

  for(var i=0; i<12; i++){
    if(months[i] === arrayDate2[1]){
      month2 = i;
    }
  }

  //day parse + insert data
  intDate1.push(parseInt(arrayDate1[0]))
  intDate1.push(month1)
  intDate1.push(parseInt(arrayDate1[2]))
  intDate2.push(parseInt(arrayDate2[0]))
  intDate2.push(month2 + 1)
  intDate2.push(parseInt(arrayDate2[2]))

  for(var i=0; i<3; i++){
    Logger.log(intDate1[i] + " ");
  }

  result = intDate1.concat(intDate2)
  
  return result;

}

// Helper Function
function compareDates(date) {
  if (date[2] >= date[5]) {
    if(date[1] >= date[4]){
      if(date[0] >= date[3])
      Logger.log("finish")
      return 0; // dates are more than or equal
    }
  }else {
    return 1; // date1 is after date2
  }
}
