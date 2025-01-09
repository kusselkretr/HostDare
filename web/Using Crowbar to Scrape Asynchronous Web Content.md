
# Using Crowbar to Scrape Asynchronous Web Content

Web scraping often involves extracting information from dynamically generated web pages. In many cases, the content you need is not available in the raw HTML but is instead loaded asynchronously via JavaScript or AJAX after the page has fully loaded. Simply downloading the HTML using tools like POCO or HttpClient won’t retrieve this data. While it’s possible to parse and execute JavaScript manually, leveraging a browser engine is a more practical solution.

Crowbar, developed by MIT's SIMILE group, is a tool that uses Firefox’s Gecko engine to execute JavaScript on web pages. After waiting for a specified duration, Crowbar serializes the DOM back into HTML, making it easier to scrape asynchronously loaded content.

---

## Why Crowbar?

The name "Crowbar" itself is symbolic—it acts as a tool to pry open web pages and retrieve content that would otherwise be hard to access. Crowbar allows you to extract content generated dynamically, similar to how a crowbar pulls out nails effortlessly.

Unfortunately, Crowbar development has been discontinued for years, and no official release version exists. While better alternatives may exist now, Crowbar can still be useful for certain use cases.

---

## System Requirements

Before using Crowbar, ensure the following requirements are met:

### XULRunner (v1.8.1+)
XUL is an XML-based user interface technology used by Mozilla to build applications like Firefox. With XULRunner, you can run programs with similar functionality to Firefox, including executing JavaScript on web pages. Crowbar is built on this framework.

Follow Mozilla’s [XULRunner Getting Started Guide](https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XULRunner) to configure XULRunner and run a basic "Hello World" program.

---

## Getting Crowbar

Crowbar doesn’t have an officially released version. The source code can be downloaded from its Subversion repository:

```plaintext
http://simile.mit.edu/repository/crowbar/trunk/
```

Alternatively, you can find modified versions of Crowbar from other sources.

---

## Running Crowbar

The official documentation provides instructions for running Crowbar on Windows, Linux, and macOS. Here’s a simple guide for Windows:

1. Open `cmd.exe` and run the following commands:
    ```plaintext
    c:\> %XULRUNNER_HOME%\xulrunner.exe –install-app %CROWBAR%\xulapp
    c:\> cd %CROWBAR%\xulapp
    c:\> %XULRUNNER_HOME%\xulrunner.exe application.ini
    ```

2. Replace `%XULRUNNER_HOME%` with the XULRunner installation directory and `%CROWBAR%` with the Crowbar file directory.

3. If successful, a window titled "Crowbar" will appear.

---

## How Crowbar Works

When Crowbar is running, it displays the last accessed URL in a small window. Its output is served via a REST-based web service, typically on port `10000`. 

To scrape a webpage, direct your browser to:

```plaintext
http://127.0.0.1:10000/?url=http://example.com/&delay=1000
```

### Parameters:
- **url**: The target URL (URL-encoded) you want to scrape.
- **delay**: The wait time (in milliseconds) before Crowbar outputs the final DOM.

Using libraries like HttpClient, you can programmatically retrieve the processed HTML by sending requests to this endpoint.

---

## Advanced Crowbar Features

Crowbar offers multiple scraping modes, though the official documentation is incomplete. To explore additional functionality, you may need to dive into the source code. Keep in mind that Crowbar is better suited for small-scale scraping projects. Its performance for large-scale scraping remains untested.

---

## Fixing Chinese Encoding Issues

When scraping Chinese web pages, Crowbar may produce garbled output due to lack of support for non-English character sets. This issue can be resolved by modifying the source code.

1. Open the file:
   ```plaintext
   %CROWBAR%\xulapp\chrome\crowbar\content\crowbar.js
   ```

2. Locate line 223 and replace the `try` block with the following code:

   ```javascript
   try {
       var charset = "UTF-8"; // Specify the desired character encoding
       var os = Components.classes["@mozilla.org/intl/converter-output-stream;1"]
                   .createInstance(Components.interfaces.nsIConverterOutputStream);
       os.init(outstream, charset, 0, 0x0000);
       os.writeString(response);
       os.close();
       instream.close();
       outstream.close();
   }
   ```

This modification ensures the output is encoded in UTF-8, resolving the encoding issue.

---

## Example Usage

To scrape dynamically loaded content, simply configure Crowbar as described and make requests to its local REST endpoint. For instance:

```plaintext
http://127.0.0.1:10000/?url=http://simile.mit.edu/&delay=1000
```

This retrieves the final DOM of the target page after waiting for one second (`delay=1000` milliseconds).

---

## Conclusion

Crowbar is a handy tool for scraping web pages with dynamically loaded content. While its development has ceased, it can still be useful for specific scenarios. By leveraging XULRunner and Firefox’s Gecko engine, Crowbar simplifies the process of executing JavaScript and retrieving rendered DOMs. However, for large-scale scraping projects, more modern and robust solutions might be better suited.
