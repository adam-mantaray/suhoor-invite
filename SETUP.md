# RSVP Backend Setup (2 minutes)

## Step 1: Create Google Sheet
1. Go to [sheets.new](https://sheets.new)
2. Name it **"Suhoor RSVP"**
3. In Row 1, add these headers:
   ```
   Timestamp | Name | Email | Phone | Guests | Dietary | Source | Investor | Notes
   ```

## Step 2: Add Apps Script
1. In the Sheet, go to **Extensions → Apps Script**
2. Delete everything and paste this:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  
  sheet.appendRow([
    new Date().toISOString(),
    data.name || '',
    data.email || '',
    data.phone || '',
    data.guests || '1',
    data.dietary || '',
    data.source || '',
    data.investor || '',
    data.notes || ''
  ]);
  
  // Send email notification
  MailApp.sendEmail({
    to: 'adam.mansour2695@gmail.com',
    subject: '🌙 New Suhoor RSVP: ' + data.name,
    body: 'Name: ' + data.name + '\nEmail: ' + data.email + '\nPhone: ' + data.phone + '\nGuests: ' + data.guests + '\nDietary: ' + (data.dietary || 'None') + '\nSource: ' + (data.source || 'N/A') + '\nInvestor Interest: ' + (data.investor || 'N/A') + '\nNotes: ' + (data.notes || 'None')
  });
  
  return ContentService
    .createTextOutput(JSON.stringify({status: 'success'}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

3. Save (Ctrl+S)

## Step 3: Deploy
1. Click **Deploy → New deployment**
2. Type: **Web app**
3. Execute as: **Me**
4. Who has access: **Anyone**
5. Click **Deploy**
6. Copy the **Web app URL**
7. Update `index.html` — replace the empty `GOOGLE_SCRIPT_URL` with your URL

Done! RSVPs will appear in the sheet + email notifications.
