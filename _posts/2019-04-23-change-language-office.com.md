I've recently joined an organization where the [office.com](https://outlook.office.com) was configured to use the French language. I found quite easily how to change the language for the mail and the one for the sharepoint it is controlled by the page itself. But for applications like Word, Excel, PowerPoint and OneDrive the situation was very strange. The application would load on my browser in English and only after openning a resource, it would switch to French including the upper commong part with the bar. This post is to explain how to resolve this issue and to share the explanation I've received from the it guys who helped me out.

Naturally, I created an IT ticket as I couldn't find a solution to my issue. After some interactions with different guys, someone provided a reasonable explanation. First he mentioned, that [office.com](https://outlook.office.com) chooses the language based on the country that the requests originates from. I currently live and work in Belgium, but my organization's proxy setup has a "random" logig and I am accessing internet as if as I was in Paris, France. So [office.com](https://outlook.office.com) decided to set up everything in French. He also explained that the [office.com](https://outlook.office.com) is devided into 3 logical applications each with its own configurations:
- Mail
- Sharepoint
- OneDrive

OneDrive includes all applications that depend on an actual file, like most office applications do. For this reason, while I had configured English as the language for the mail application, I was still getting Office applications in French because the configuration is not shared. Strangely enough, the UI option to enter the settings page was missing or maybe it is not available by choice of Microsoft so the technician help me created the url for the language settings page. 

The url is like this `https://brusselsairlines-my.sharepoint.com/personal/{sortofemail}/_layouts/15/muisetng.aspx` where the **{sortofemail}** is in essence a "url friendly" version of your email. To find your own, just naviagate to the OneDrive application and extract your **{sortofemail}** from the url on your browser address bar. It should look like this `https://brusselsairlines-my.sharepoint.com/personal/{sortofemail}/_layouts/15/onedrive.aspx`.