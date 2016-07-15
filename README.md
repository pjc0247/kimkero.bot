# kimkero.bot
keroro fun

```cs
using System.Linq;
using System.Xml;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.ServiceModel.Syndication;

static HashSet<string> posts = new HashSet<string>();

[Bootstrap]
public void OnInit() {
    string url = "http://kimkero.tistory.com/rss";
    XmlReader reader = XmlReader.Create(url);
    SyndicationFeed feed = SyndicationFeed.Load(reader);
    reader.Close();

    foreach (SyndicationItem item in feed.Items)
    {
        string subject = item.Title.Text;    
        string link = item.Links.First().Uri.ToString();
    
        posts.Add(link);
        Console.WriteLine(link);

//        Slack.SendMessage("#p_slackbot", item.Title.Text);
    }
}

[Schedule(11)]
public void OnUpdate() {
    string url = "http://kimkero.tistory.com/rss";
    XmlReader reader = XmlReader.Create(url);
    SyndicationFeed feed = SyndicationFeed.Load(reader);
    reader.Close();

    foreach (SyndicationItem item in feed.Items)
    {
        string subject = item.Title.Text;    
        string link = item.Links.First().Uri.ToString();

        if (posts.Contains(link))
            continue;

        posts.Add(link);
        Console.WriteLine(link);

        Slack.SendMessage("#keroro", subject);
        Slack.SendMessage("#keroro", link);
    }
}
```
