<p align="center">
  <img src="html.png" />
</>

#  Semantic HTML 

There are hundreds of semantic tags available to help describe the meaning of your HTML documents. Below is a cheat sheet with some of the most common ones you’ll use in this course and in your development career.

# Sectioning tags

Use the following tags to organize your HTML document into structured sections. 

**&lt;header&gt;**

The header of a content section or the web page. The web page header often contains the website branding or logo.

**&lt;nav&gt;**

The navigation links of a section or the web page.



**&lt;footer&gt;**

The footer of a content section or the web page. On a web page, it often contains secondary links, the copyright notice, privacy policy and cookie policy links.



**&lt;main&gt;**

Specifies the main content of a section or the web page.



**&lt;aside&gt;**

A secondary set of content that is not required to understand the main content.



**&lt;article&gt;**

An independent, self-contained block of content such as a blog post or product.



**&lt;section&gt;**

A standalone section of the document that is often used within the body and article elements.



**&lt;details&gt;**

A collapsed section of content that can be expanded if the user wishes to view it.



**&lt;summary&gt;**

Specifies the summary or caption of a **&lt;details&gt;** element.



**&lt;h1&gt;****&lt;h2&gt;****&lt;h3&gt;****&lt;h4&gt;****&lt;h5&gt;****&lt;h6&gt;**

Headings on the web page. **&lt;h1&gt;** indicates the most important heading whereas **&lt;h6&gt;** indicates the least important. 
# Content tags

**&lt;blockquote&gt;**

Used to describe a quotation.



**&lt;dd&gt;**

Used to define a description for the preceding **&lt;dt&gt;** element.



**&lt;dl&gt;**

Used to define a description list.



**&lt;dt&gt;**

Used to describe terms inside **&lt;dl&gt;** elements.



**&lt;figcaption&gt;**

Defines a caption for a photo image.



**&lt;figure&gt;**

Applies markup to a photo image.



**&lt;hr&gt;**

Adds a horizontal line to the parent element.



**&lt;li&gt;**

Used to define an item within a list.



**&lt;menu&gt;**

A semantic alternative to **&lt;ul&gt;** tag.



**&lt;ol&gt;**

Defines an ordered list.



**&lt;p&gt;**

Defines a paragraph.



**&lt;pre&gt;**

Used to represent preformatted text. Typically rendered in the web browser using a monospace font.



**&lt;ul&gt;**

Unordered list

# Inline tags

**&lt;a&gt;**

An anchor link to another HTML document.



**&lt;abbr&gt;**

Specifies that the containing text is an abbreviation or acronym.



**&lt;b&gt;**

Bolds the containing text. When used to indicate importance use **&lt;strong&gt;** instead.



**&lt;br&gt;**

A line break. Moves the subsequent text to a new line.



**&lt;cite&gt;**

Defines the title of creative work (for example a book, poem, song, movie, painting or sculpture). The text in the **&lt;cite&gt;** element is usually rendered in italics.



**&lt;code&gt;**

Indicates that the containing text is a block of computer code.



**&lt;data&gt;**

Indicates machine-readable data.



**&lt;em&gt;**

Emphasizes the containing text.



**&lt;i&gt;**

The containing text is displayed in italics. Used to indicate idiomatic text or technical terms.



**&lt;mark&gt;**

The containing text should be marked or highlighted.



**&lt;q&gt;**

The containing text is a short quotation.



**&lt;s&gt;**

Displays the containing text with a strikethrough or line through it.



**&lt;samp&gt;**

The containing text represents a sample.



**&lt;small&gt;**

Used to represent small text, such as copyright and legal text.



**&lt;span&gt;**

A generic element for grouping content for CSS styling.



**&lt;strong&gt;**

Displays the containing text in bold. Used to indicate importance.



**&lt;sub&gt;**

The containing text is subscript text, displayed with a lowered baseline.



**&lt;sup&gt;**

The containing text is superscript text, displayed with a raised baseline.



**&lt;time&gt;**

A semantic tag used to display both dates and times.



**&lt;u&gt;**

Displays the containing text with a solid underline.



**&lt;var&gt;**

The containing text is a variable in a mathematical expression.

# Embedded content and media tags

**&lt;audio&gt;**

Used to embed audio in web pages.



**&lt;canvas&gt;**

Used to render 2D and 3D graphics on web pages.



**&lt;embed&gt;**

Used as a containing element for external content provided by an external application such as a media player or plug-in application. 



**&lt;iframe&gt;**

Used to embed a nested web page. 



**&lt;img&gt;**

Embeds an image on a web page.



**&lt;object&gt;**

Similar to **&lt;embed&gt;** but the content is provided by a web browser plug-in.



**&lt;picture&gt;**

An element that contains one **&lt;img&gt;** element and one or more **&lt;source&gt;** elements to offer alternative images for different displays/devices.



**&lt;video&gt;**

Embeds a video on a web page.



**&lt;source&gt;**

Specifies media resources for **&lt;picture&gt;**, **&lt;audio&gt;** and**&lt;video&gt;** elements.

**&lt;svg&gt;**

Used to define Scalable Vector Graphics within a web page.

# Table tags

**&lt;table&gt;**

Defines a table element to display table data within a web page.



**&lt;thead&gt;**

Represents the header content of a table. Typically contains one **&lt;tr&gt;** element.



**&lt;tbody&gt;**

Represents the main content of a table. Contains one or more **&lt;tr&gt;**elements.



**&lt;tfoot&gt;**

Represents the footer content of a table. Typically contains one **&lt;tr&gt;** element.



**&lt;tr&gt;**

Represents a row in a table. Contains one or more **&lt;td&gt;** elements when used within **&lt;tbody&gt;** or **&lt;tfoot&gt;**. When used within **&lt;thead&gt;**, contains one or more **&lt;th&gt;** elements.



**&lt;td&gt;**

Represents a cell in a table. Contains the text content of the cell.



**&lt;th&gt;**

Defines a header cell of a table. Contains the text content of the header.



**&lt;caption&gt;**

Defines the caption of a table element.



**&lt;colgroup&gt;**

Defines a semantic group of one or more columns in a table for formatting.



**&lt;col&gt;**

Defines a semantic column in a table.


# HTML **&lt;meta&gt;** tags 

Earlier in the course, you learned about meta tags and how you can leverage them to convey information to search engines to better categorize your pages. We recommend that you keep this cheat sheet handy when building your web applications. The structure of a meta tag is as follows.

**Name**

The name of the property can be anything you like, although browsers usually expect a value they understand and can take an action upon. An example would be **&lt;meta name="author" content="name"&gt;** to state the author of the page. 



**Content** 


The content field specifies the property's value. For example, you can use **&lt;meta name="language" content="english"&gt;**, to specify the language of the webpage to search engines. 

**Charset** 


The charset is a special field that lets you specify the character encoding used for the page so that the browser can display it properly. The most frequently used is utf-8, and you would add it to your HTML header as follows: **&lt;meta charset="UTF-8"&gt;**  

**HTTP-equiv** 


This field stands for HTTP equivalent, and it’s used to simulate HTTP response headers. This is rare to see, and it’s recommended to use HTTP headers over HTML http-equiv meta tags. For example, the next tag would instruct the browser to refresh the page every 30 minutes: **&lt;meta http-equiv="refresh" content="30"&gt;** 

Basic meta tags (meta tags For SEO) 

**&lt;meta name="description"/&gt;** provides a brief description of the web page 



**&lt;meta name=”title”/&gt;** specifies the title of the web page 



**&lt;meta name="author" content="name"&gt;** specifies the author of the web page  



**&lt;meta name="language" content="english"&gt;** specifies the language of the web page 





**&lt;meta name="robots" content="index,follow" /&gt;** tells search engines how to crawl or index a certain page 



**&lt;meta name="google"/&gt;** tells Google not to show the sitelinks search box for your page when showing search results 



**&lt;meta name="googlebot" content=”notranslate” /&gt;** tells Google you don’t want to provide an automatic translation for your page if the user uses a different language  



**&lt;meta name="revised" content="Sunday, July 18th, 2010, 5:15 pm" /&gt;** specifies the last modified date and time on which you have made certain changes 



**&lt;meta name="rating" content="safe for kids"&gt;** specifies the expected audience for your page 



**&lt;meta name="copyright" content="Copyright 2022"&gt;** specifies a Copyright 

**&lt;meta http-equiv="..."/&gt;** tags

 **&lt;meta http-equiv="content-type" content="text/html"&gt;** specifies the format of the document returned by the server 



**&lt;meta http-equiv="default-style"/&gt;**  specifies the format of the styling document 



**&lt;meta http-equiv="refresh"/&gt;** specifies the duration of the page before it’s considered stale 



**&lt;meta http-equiv=”Content-language”/&gt;** specifies the language of the page 



**&lt;meta http-equiv="Cache-Control" content="no-cache"&gt;** instructs the browser how to cache your page 
Responsive design/mobile meta tags 

**&lt;meta name="format-detection" content="telephone=yes"/&gt;** indicates that telephone numbers should appear as hypertext links that can be clicked to make a phone call 



**&lt;meta name="HandheldFriendly" content="true"/&gt;** specifies that the page can be properly visualized on mobile devices 



**&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"/&gt;** specifies the area of the window in which web content can be see