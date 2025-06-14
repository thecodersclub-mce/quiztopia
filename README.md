---
---

# 🧠 MINDMASH Quiz, Feedback, & Leaderboard Application

This repository contains the front-end code for the **MINDMASH** application, designed by The Coders Club. It features an interactive quiz, a feedback form, and a dynamic leaderboard, all capable of adapting their state and submitting data.

---

## ✨ Features

* **Interactive Quiz Form**: Allows users to answer multiple-choice, multi-select, and open-ended questions.
* **Feedback Form**: Gathers valuable insights from users with various input types.
* **Dynamic Form Control**: Both quiz and feedback forms can be set to `enable` or `disable` using a simple JavaScript variable for active control.
* **"Quiz Closed" Experience**: When disabled, the quiz section's questions are **jumbled and blurred**, and input fields are inaccessible, providing clear visual feedback that it's currently unavailable.
* **Submission Success Animation**: A pleasant tick mark animation appears upon successful form submission.
* **Google Sheets Integration**: Submits quiz and feedback form data directly to Google Sheets using a Google Apps Script web app (requires user setup).
* **Dynamic Leaderboard Display**:
    * **Weekly Tabs**: View quiz results categorized by different weeks.
    * **Department-wise Filtering**: Within each week, results can be filtered by specific departments (e.g., CSE 2nd Year, CSE (AI/ML) 2nd Year, CSE 3rd Year).
    * Clearly shows S.No., Name, and Score.
* **Responsive Design**: Adapts to different screen sizes for optimal viewing on desktop and mobile devices.
* **Anti-Copying Measures**: Basic CSS to prevent text selection in certain quiz elements when disabled.

---

## 🚀 Setup & Usage

### 1. Google Apps Script Setup (Backend for Data Submission)

Both the quiz and feedback forms submit data to a Google Sheet via a deployed Google Apps Script web app. You'll need to set this up first for submissions to work.

#### For each form (Quiz and Feedback):

1.  **Create a New Google Sheet**: Go to Google Sheets and create a new blank spreadsheet.
2.  **Rename the First Tab**: Rename the first tab (e.g., `Sheet1`) to match the expected column names for the respective form.
    * **For Quiz Form**: The first row of your Google Sheet should contain these column headers: `Name`, `Email`, `Department`, `Year`, `Question1`, `Question2`, `Question3`, `Question4`, `Question5`.
    * **For Feedback Form**: The first row of your Google Sheet should contain these column headers: `Name`, `Email`, `Department`, `Year`, `SatisfactionRating`, `LikedMost`, `Suggestions`, `Recommend`, `OtherComments`.
3.  **Open Apps Script**: In your Google Sheet, go to `Extensions > Apps Script`.
4.  **Paste the Script**: Delete any existing code (`Code.gs`) and paste the following Google Apps Script code. This is a generic script that will work for both forms, assuming your column headers match.

    ```javascript
    function doPost(e) {
      var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
      var headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0];
      var rowData = [];

      // Add a timestamp as the first column (optional, but good practice)
      rowData.push(new Date());

      for (var i = 0; i < headers.length; i++) {
        var header = headers[i];
        if (header.toLowerCase() === 'timestamp') {
          // Skip if timestamp is already added
          continue;
        }
        var value = e.parameter[header];
        rowData.push(value || ''); // Add value, or empty string if not present
      }

      sheet.appendRow(rowData);

      return ContentService.createTextOutput(JSON.stringify({"result":"success", "row": sheet.getLastRow()})).setMimeType(ContentService.MimeType.JSON);
    }
    ```
5.  **Deploy as Web App**:
    * Save the script (`File > Save project`).
    * Click `Deploy > New deployment`.
    * For "Select type," choose **Web app**.
    * For "Execute as," choose `Me`.
    * For "Who has access," choose `Anyone`.
    * Click `Deploy`.
    * **Copy the Web app URL**. This is crucial!

### 2. Frontend File Setup

1.  **Download/Clone**: Get all the HTML, CSS (`styles.css`), and image files (`TCC.png`, `MCE_tab.png`, `IG.png`, `LINKEDLN.png`, `YT.png`, `WP.svg`) from this repository.
2.  **Update Script URLs**:
    * Open `index.html` (for the Quiz form) in a text editor.
    * Find the line: `const scriptUrl = 'YOUR_QUIZ_GOOGLE_APPS_SCRIPT_URL_HERE';`
    * Replace the placeholder URL with the **Web app URL** you copied from your Google Apps Script deployment for the Quiz form's Google Sheet.
    * Do the **same for `feedback.html`**, replacing its `scriptUrl` with the **Web app URL** for the Feedback form's Google Sheet.

### 3. Dynamic Form Status Control

* **For the Quiz form (`index.html`)**:
    * Open `index.html`.
    * Find the line: `const linkControlStatus = "disable";`
    * Change `"disable"` to `"enable"` to make the quiz form active.
* **For the Feedback form (`feedback.html`)**:
    * Open `feedback.html`.
    * Find the line: `const formControlStatus = "disable";`
    * Change `"disable"` to `"enable"` to make the feedback form active.

### 4. Leaderboard Data Management

The leaderboard data in `leaderboard.html` is currently **static HTML**. This means that to update the quiz scores, you will need to manually edit the `leaderboard.html` file.

* To update scores for Week 1, find the `<div id="week1" ...>` section.
* Within that, locate the specific department's `div` (e.g., `<div id="cse2_week1" ...>`).
* Edit the `<table>` rows (`<tr><td>...</td></tr>`) to reflect the latest rankings and scores.
* Follow the same process for other weeks and departments as needed.

---

## 💻 Local Development

You can open `index.html`, `feedback.html`, and `leaderboard.html` directly in your web browser to test them. Remember that for form submissions to work, you *must* have completed the Google Apps Script setup and updated the `scriptUrl` in `index.html` and `feedback.html`.

---

## 📄 File Structure

* `index.html`: The main Quiz application page.
* `feedback.html`: The Feedback form page.
* `leaderboard.html`: Displays the quiz results with weekly and department-wise filtering.
* `styles.css`: Contains the common CSS styling for all pages.
* `TCC.png`, `MCE_tab.png`, `IG.png`, `LINKEDLN.png`, `YT.png`, `WP.svg`: Image assets used throughout the pages.

---

## 🤝 Contributing

Feel free to fork this repository, open issues, or submit pull requests.

---

## 📝 License

This project is open-sourced under the MIT License.

---

## 📧 Contact

For any questions or suggestions, please contact The Coders Club.

---
