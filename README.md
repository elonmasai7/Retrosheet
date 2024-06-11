# Turn Google Spreadsheet to JSON Endpoint (for Android and JVM) for FREE

## Overview

This guide provides step-by-step instructions on how to convert a Google Spreadsheet into a JSON endpoint that can be easily used within Android and JVM applications. Utilizing Google Sheets as a backend for your application can simplify data management and offer a cost-effective solution. This approach is free and leverages Google Sheets' API to fetch data in JSON format.

## Prerequisites

1. A Google account.
2. A Google Spreadsheet with your data.
3. Basic knowledge of Android or JVM development.
4. Internet connection.

## Steps

### 1. Prepare Your Google Spreadsheet

1. **Create a Google Spreadsheet**:
   - Go to [Google Sheets](https://sheets.google.com).
   - Create a new spreadsheet or open an existing one.
   - Ensure your data is structured properly with headers in the first row.

2. **Publish the Spreadsheet to the Web**:
   - Click on `File` > `Share` > `Publish to the web`.
   - Choose the sheet you want to publish.
   - Click on `Publish`.
   - Copy the published link.

### 2. Configure Google Sheets API

1. **Set Up Google Cloud Project**:
   - Go to [Google Cloud Console](https://console.cloud.google.com/).
   - Create a new project.
   - Enable the Google Sheets API for your project.
   - Create credentials (API Key).

2. **Share Spreadsheet**:
   - Ensure your Google Spreadsheet is shared with the service account email if you're using OAuth 2.0, or set it to `Anyone with the link` if you're using a public API key.

### 3. Fetch Data as JSON

1. **Construct the JSON URL**:
   - Format: `https://sheets.googleapis.com/v4/spreadsheets/{SPREADSHEET_ID}/values/{SHEET_NAME}?key={API_KEY}`
   - Replace `{SPREADSHEET_ID}` with your spreadsheet ID (found in the URL).
   - Replace `{SHEET_NAME}` with the name of the sheet you want to access.
   - Replace `{API_KEY}` with your API key from Google Cloud Console.

### 4. Integrate with Android or JVM Application

#### For Android

1. **Add Dependencies**:
   - Open your `build.gradle` file and add the following dependencies:
     ```groovy
     implementation 'com.google.api-client:google-api-client-android:1.32.2'
     implementation 'com.google.apis:google-api-services-sheets:v4-rev614-1.25.0'
     implementation 'com.squareup.okhttp3:okhttp:4.9.3'
     ```

2. **Fetch Data Using HTTP Request**:
   - Use OkHttp or any other HTTP client to make a request to your JSON URL.
   - Example using OkHttp:
     ```java
     OkHttpClient client = new OkHttpClient();

     String url = "https://sheets.googleapis.com/v4/spreadsheets/{SPREADSHEET_ID}/values/{SHEET_NAME}?key={API_KEY}";

     Request request = new Request.Builder()
             .url(url)
             .build();

     client.newCall(request).enqueue(new Callback() {
         @Override
         public void onFailure(Call call, IOException e) {
             e.printStackTrace();
         }

         @Override
         public void onResponse(Call call, Response response) throws IOException {
             if (response.isSuccessful()) {
                 String jsonData = response.body().string();
                 // Parse and use the JSON data
             }
         }
     });
     ```

#### For JVM

1. **Add Dependencies**:
   - Ensure you have the necessary dependencies for HTTP requests and JSON parsing.
     ```groovy
     implementation 'com.google.api-client:google-api-client:1.32.2'
     implementation 'com.google.apis:google-api-services-sheets:v4-rev614-1.25.0'
     implementation 'com.squareup.okhttp3:okhttp:4.9.3'
     ```

2. **Fetch Data Using HTTP Request**:
   - Use OkHttp or another HTTP client to make a request to your JSON URL.
   - Example using OkHttp in a JVM application:
     ```java
     OkHttpClient client = new OkHttpClient();

     String url = "https://sheets.googleapis.com/v4/spreadsheets/{SPREADSHEET_ID}/values/{SHEET_NAME}?key={API_KEY}";

     Request request = new Request.Builder()
             .url(url)
             .build();

     try (Response response = client.newCall(request).execute()) {
         if (response.isSuccessful() && response.body() != null) {
             String jsonData = response.body().string();
             // Parse and use the JSON data
         }
     } catch (IOException e) {
         e.printStackTrace();
     }
     ```

### 5. Parse JSON Data

- Use a JSON parsing library such as `Gson` or `Jackson` to parse the JSON data fetched from the Google Sheets API.
  - Example with Gson:
    ```java
    Gson gson = new Gson();
    YourDataClass data = gson.fromJson(jsonData, YourDataClass.class);
    ```

## Conclusion

By following these steps, you can easily convert a Google Spreadsheet into a JSON endpoint and integrate it into your Android or JVM application. This method provides a simple and free solution for managing and accessing data, making it ideal for small to medium-sized projects.
