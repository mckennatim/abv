### 25/12/7

I see our account in Hostpapa as #476232. 
hiel M. 
10:05 AM

Hi Sandy,

You are right, hostpapa is the registrar for abvchorus.org. I 'chatted' with them (Raphiel.M) this morning. They said...

      "Yes, whatever she wants. If she preferred chat, email or ticket. Just sent her the request what the support needs to do and she can just copy and paste it and send it over to us. Then we will verify the account and add those DNS records"

It is probably easiest to do it through chat on the domains page https://www.hostpapa.com/domains/

Paste this in chat box...
```
Please redirect abvchorus.org to 107.172.72.246. Also please add the the fiollowing subdomains to the domain record
* alpha.abvchorus.org 
* beta.abvchorus.org 
* blog.abvchorus.org
* circleboston.abvchorus.org 
* forum.abvchorus.org 
* yiddish.abvchorus.org
```
I am happy to be on zoom with you when you do this. Otherwise:

Let me know how this goes and when you did it. I have to make server changes as soon as the redirection takes effect, 1-4 hours after you make the change. The site might be down for a little bit until the new secure letsencrypt certificate is active.  

Thanks

12/10/25

Hi Margery,

I have arranged with Sandy and the domain registrar to redirect abvchorus.org to the new 107.172.72.246 server. I am ready and depending on when Sandy is available it could get done today or tomorrow.  I am inclined to try to schedule it with Sandy for tomorow at 10:00 AM. That may change based on her availability. I can let you know when she lets me know.

In the interim we should freeze all changes to the old site. Then I can make a final update to the server today so it will be in sync with the old server. 

Once Sandy verifies the time of transfer, we should probably send out an email alerting everyone to partial disruption of service. Could you do that? It takes from 15 minutes to 3-4 hours for the new DNS record to propogate through the internet. As soon as it does, I need to establish a new https (letsencrypt) certificate for the new server. In the time between that taking effect http://abvchorus.org will work to the new site but https://abvchorus.org will not yet work. 

We could just say the site is down in the interim or we could say "The chorus website will be moving to a new server startinng at Thursday? at xx:00 AM? While the transition is taking place you may be able to reach it using http://abvchorus.org instaed of https://abvchorus.org" Whatever you think is best.

Let me know if you have any concerns before I schedule with Sandy.





Sandy@circleboston.org
abvchorus.org


hosting service: hostpapa  https://hostpapa.com

domain registrar: Tucows Domains Inc. https://www.tucows.com

Hi Sandy,

We are ready to switch to the new server for abvchorus.org. In order to do that I need admin access to the abvchorus.org records of the domain registrar. That might be https://www.tucows.com or maybe https://hostpapa.com has it own domain registrar. With access to the records I can re-direct abvchorus.org to 107.172.72.246 and add a couple of subdomains as well. 

Typically you have to renew your domain name periodically by paying a fee. If that is the case I just need the login credentials that you use to do that. It is possible it is done for you by hostpapa or whatever the hosting service is. Then, I would need the login credentials for them. 

Most of them now use two factor authentication which does complicate thing as they send a code to the email on record and you have 15 minutes to enter it to gain access.

I can ask them the best way to go about the transfer if I know which company you have been dealing with.

Thanks for your cooperation in this. 




### 25/12/6

Hi Margery,

I did some work on the new server and the chorus website is replicated and running there: [http://107.172.72.246](http://107.172.72.246)

If you could check and see if you can connect there with winscp that would be great. 

    host: 107.172.72.246
    username: abv
    password: abeserevelt10

See if your editing tools will work. We will need them as site editing option for you until such time as https://abvchorus.org/admin page is live and you decide to edit that way. 


I will be in touch with Sandy to get us access to the domain registrar. If everything is working on your end, I will make the move to redirect abvchorus.org to 107.172.72.246. 

We will still have access to the old server for as long as we want to archive whatever else is there onto the new site.

I would like to do this rather expeditiously. Derek is already rolling out the new repetoire and keeping the the existing updated AND the new site updated durung the transfer will be a bit tedious.

To that end I ask that from today until the transfer is done, we keep a shared list of what we have changed on the existing site so we don't lose those changes during the transfer.

Thakns for your tolerance and willing to go through these changes. Things will be better and administering the site will be easier down the road.


## racknerd login ABVchorus.org
- username: info@circleboston.org
- pwd: ABVchorus10


NerdVM Panel URL: https://nerdvm.racknerd.com/
- Username: vmuser265848 
- pwd ABVchorus10

root
- 107.172.72.246 
- pwd : abeserevelt10
     
home/abv
- Username: abv (used to be abvch0)
- pwd: abeserevelt10

## Discussions with Margery

----

Yes, I can do that. My biggest challenge is paying attention to my emails. 

Regarding changes, I think the only structural change was to add a new file: calendar.html. The index.html now just has chorus info stuff in it. I am thinking your system with the html editor will still work on calendar.html.

I would love it if we could continue to work together. The changes beneath the basic structure make the pages responsive, easier to read on the phone. The html is all cleaned up. Styles are pulled out into styles.css. The calendar remains a table, again with styles separate from the html tags. 

These changes will allow us to make the updating process much easier, quicker and cleaner allowing us to transition at whatever pace we are each comfortable with. I have already written some code that I use when I update. It is in its initial stages but I am using that code already to make my updates and to do things like automatically convert .docx to pdf, and clean up the file names so they can be more easily viewed and processed.

Perhaps sometime this late fall or winter we could meet and talk over our collaboration

----

Margery Meadow
Nov 8, 2025, 11:21 PM (13 hours ago)
to me

Guess I was looking in the wrong folder for the reorganized files. Thanks for fixing the notes link. I can see that you are refining the layout (double-spaces have returned to the "complete" page, for example).

I am not sure that collaboration will work, especially since I sense you want the freedom to reorganize and use tools you are developing. For example, I had originally chosen to put the schedule on the main page, because it is the most important information; now members have to dig down underneath for it. Not a big deal; I don't hate the new structure and do not mind if you keep it, but you did make that change without collaboration. Also, whatever methods you implement, it's important that the site can continue to be maintained by someone without your tools.

- It is important to make changes fairly quickly.  I found if I did not do so, I would fall behind and get confused; maybe you can keep track better than I! Chorus members also get upset if they cannot find music and schedule updates. (Sadly, I think some of them do not know how to search their email.) Talking about updates: could you add that translation file for Lo Yare?

- The MONTHLY CALENDAR and WEEKLY CALENDAR headings do not render that well; they stack up on my phone or on my computer if I reduce window size. (Maybe you don't need them?)

- To tidy things up, I deleted the unused folder rehearsal_notes_2025_2026.

- I assumed that you were done making changes for today, so as a test I edited the calendar file and corrected errors in the schedule. I needed to go edit the source to make some changes because I was unable to make the edits in the WYSIWYG editor, possibly because of the new detailed HTML date classes? 

MM
----

Tim McKenna <mckenna.tim@gmail.com>
10:54 AM (2 hours ago)
to Margery

The columns stack as part of responsive design, the columns stack for small screen sizes. Instead of seeing all the columns and tiny text.when viewing on the phone they stack, since scrolling down on phones is easy and the text and links are easier to view/tap.

As for tools to make editing easier, they can also (eventually be hosted on the site as an admin page.

----

Margery Meadow
11:34 AM (1 hour ago)
to me

Sometimes I am directed to update the site with new info such as a schedule change, and ALSO to announce it to the chorus. I prefer that the person with the info simply send it to the chorus themselves, and trust that I would know to quickly update the website upon reading. Someone was telling members not to use the main email list under any circumstances--that it was only for Derek, chorus committee members; I made a point of disagreeing but the belief is still out there. Maybe that's why Laura did not send it herself. Sometimes it has been easier for me to just do the announcing than push back. You got the December 14 change on the website correctly, however I made a mistake in my announcement--social time is before rehearsal time--and now have to send a correction!

Clarifying your comment
   Yes, I can do that. My biggest challenge is paying attention to my emails. 

So just making completely sure--you are willing to be the main person for the website? I would be in favor, because after creating it 14-15 years ago and maintaining it (as you can see, I use that term loosely!) I am seriously burnt out. In addition to the website, I have been doing the email list maintenance; although that's only a big chore early in the season. Repeating a few of my related thoughts:

- The community expects updates to be made promptly. We could reduce those expectations, but I think it's a good practice. As I mentioned, many members don't seem to know how to search their email. And they certainly don't know that all emails are stored at https://groups.google.com/ .
- I am on all the email lists, so I see audio files etc. that are sent to the individual sections rather than the main list. I can forward those to you, or put you on the section lists as well. Or something else we haven't thought of!
- I look forward to learning your methods so I can be an effective backup. Every few years, I would search for a way to clean up the increasingly horrendous source code, but never got very far.

Margery
----



Tim McKenna <mckenna.tim@gmail.com>
12:47 PM (35 minutes ago)
to Margery

Margery,
Sure I am happy to be the main person and would appreciate it if you were the backup.

As I worked on the site, and even before, I was continually impressed by what a cool resource it has been for ABV for all these years. You have done an amazing job of creating a consistently clear and useful web presentation that has worked well for everybody. You have created a website that is a core and important part of ABV.

 I am going to have to up my email game and improve my politeness moving forward, but I am guessing that will be good for me. 

My secret agenda is to give you a well deserved break, while I, on and off, work on the editing tools. At the point when they are presentable and debugged, I intend to lure you back in as co-admin:)

Tim
----

Tim McKenna <mckenna.tim@gmail.com>
1:16 PM (4 minutes ago)
to Margery

BTW
Who administers abvchorus.org's domain and host? Who do you talk to if you need approval to do something? 

Margery Meadow
12:29 PM (2 hours ago)
to sandy@circleboston.org, me

Hi, Sandy and Tim. Tim and I will be discussing the direction of the chorus website. I would wait on any effort to move abvchorus.org to a faster host.

Margery


Margery Meadow
12:31 PM (2 hours ago)
to me

Tim, probably we should talk more, maybe in person or a virtual video discussion. I am unclear on your ideas for maintaining the site. Whatever we do, a primary goal is to keep it maintainable by a person without extensive technical skills, and I wonder if we are moving away from that.

Margery


Sandy Martin
1:45 PM (55 minutes ago)
to Margery, me

I'm fine with the type of changes Tim is suggesting. We will need to coordinate for passwords and admin ownership; possibly needing to be online together to register everyone. 
Once the 2 of you are confident about what changes you want, let me know and we'll work on it. 
thanks,
Sandy
Sandy Martin

Director of Operations

Regular hours: Mon-Thurs, 10-5pm



BOSTON WORKERS CIRCLE

Center for Jewish Culture & Social Justice


6 Webster Street Brookline, MA 02446

617.566.6281 | www.circleboston.org | Find us on Facebook

Margery,

Sure, I prefer to meet in person. I agree there are things we should discuss. I am out of town Wednesday till Sunday early. So perhaps after the concert would be best. 

To be clear, it is not just about a faster host, but a better host that would give ABV the access needed to maintain the site and make upgrades less onerous. A person with technical skills (me, if we can come to some agreement on this), would be a person who creates tools to allow a person without technical skills to easily update the site. You can't do that on the very limited hosting environment the site is currently in. 

Once those tools are created and hosted on a new server, no technical skills would be needed to use them. The site that ABV members will see will still be just static HTML pages with links, just like it is today. You would not even need to use the new tools, you could use winSCP and KompoZer, but frankly I wouldn't wish that on anyone.

I do have some proposals, in the short term, that I would like you to consider. 

1. Modify where resources are stored for songs ( for new additions to our repertoire). It makes better sense from a data processing point of view, not to have files for a song stored in more than one subdirectory. The file extension tells us what kind of file it is.
     *  all section leader audio, other audio, other video, sheet music and translations will go in the same song subfolder we could call it 'songs' or 'lids'. The subdirectory would be derived from the song name as you have been doing but without spaces and non alphanumeric characters. ex: for 'A new song's name' would be stored in 
        *  'resources/lids/a_new_songs_name/bass_listen_and_repeat.mp3' 
        *  'resources/lids/a_new_songs_name/sheet_music.pdf'
     * existing files would not need to move, their links would still work
     * How ABV users see the song entry in 'music-complet.html' would remain the same, grouped under 'Audio' or whatever.
     * it will always be obvious where the files are stored, the links in the page will tell you.
     * all links in calendar can remain in `resources/other/rehearsal_notes/25-26/xxx

Here is a draft agenda for our meeting:

* discuss your concerns about our collaboration
* discuss my concerns about our collaboration
* consider my proposal for file storage locations
* discuss the idea of eventually moving to a new server
* discuss your concerns about what adding admin tools would mean for a future without a tech-skilled admin.
* go over the process of using KompoZer to edit the HTML-5 version of the code for calendar.html and music-complete.html. (we will need our computers to do this)

I hope that we can come to an agreement that allows for both short-term continuation of my involvement and long term joint improvement to the site.
  
All the best,<br>
Tim

----
mon 11/17

Hi Margery,
I am back from my trip to Seattle and feeling good from the concert last night. 

I would like to meet in person, anywhere you would like. I am pretty available the rest of this week and next except
* Tuesday 10:30 - 3:30
* Thursday until 2



## Issues moving forward

* limitations of current hosting service
* recorded performances and archive
  * enormous file size
  * let youtube host them
  * have the ability to create song-segments from old performance recordings 
* how to collect suggestions for future site improvements
* dealing with experimental site features - beta site


## site info

$ ip=$(curl -sS -o /dev/null -w '%{remote_ip}' https://abvchorus.org); echo "IP: $ip"; echo "=== DNS (nslookup) ==="; nslookup abvchorus.org || true; echo; echo "=== HTTP headers ==="; curl -sI https://abvchorus.org || true; echo; echo "=== RDAP WHOIS (domain) ==="; curl -s https://rdap.org/domain/abvchorus.org || true; echo; echo "=== IP owner (ipinfo) ==="; curl -s https://ipinfo.io/$ip || true; echo; echo "=== Traceroute ==="; (traceroute -n $ip 2>/dev/null || tracert.exe -d $ip) || true
IP: 204.44.192.17
=== DNS (nslookup) ===
Server:  Fios_Quantum_Gateway.fios-router.home
Address:  192.168.1.1

Non-authoritative answer:
Name:    abvchorus.org
Address:  204.44.192.17


=== HTTP headers ===
HTTP/1.1 200 OK
Date: Mon, 10 Nov 2025 02:34:10 GMT
Server: Apache
Upgrade: h2,h2c
Connection: Upgrade
Last-Modified: Sun, 02 Nov 2025 02:25:45 GMT
Accept-Ranges: none
Vary: Accept-Encoding,User-Agent
Content-Length: 5501
Content-Type: text/html


=== RDAP WHOIS (domain) ===

=== IP owner (ipinfo) ===
{
  "ip": "204.44.192.17",
  "hostname": "s105.servername.online",
  "city": "Los Angeles",
  "region": "California",
  "country": "US",
  "loc": "34.0522,-118.2437",
  "org": "AS23273 HostPapa",
  "postal": "90012",
  "timezone": "America/Los_Angeles",
  "readme": "https://ipinfo.io/missingauth"
}
=== Traceroute ===

Tracing route to 204.44.192.17 over a maximum of 30 hops

  1     2 ms     2 ms     2 ms  192.168.1.1
  2    10 ms     7 ms     5 ms  173.76.23.1 
  3    17 ms     7 ms     7 ms  100.41.25.230 
  4     *        *        *     Request timed out.
  5     *        *        *     Request timed out.
  6    13 ms    14 ms    14 ms  154.54.40.154 
  7    19 ms    17 ms    19 ms  154.54.170.69 
  8    30 ms    31 ms    30 ms  154.54.169.178 
  9    47 ms    46 ms    46 ms  154.54.29.134 
 10    49 ms    50 ms    59 ms  154.54.40.249 
 11    60 ms    58 ms    59 ms  154.54.165.26 
 12    77 ms    75 ms    74 ms  154.54.166.58 
 13    75 ms    72 ms    72 ms  154.54.44.86 
 14    71 ms    74 ms    74 ms  154.54.29.34 
 15    74 ms    73 ms    77 ms  154.24.87.118 
 16    76 ms    74 ms    75 ms  38.104.230.199 
 17    73 ms    74 ms    73 ms  104.168.110.174 
 18    73 ms    73 ms    73 ms  204.44.192.17 

Trace complete.

$ ping abvchorus.org

Pinging abvchorus.org [204.44.192.17] with 32 bytes of data:
Reply from 204.44.192.17: bytes=32 time=76ms TTL=56
Reply from 204.44.192.17: bytes=32 time=76ms TTL=56
Reply from 204.44.192.17: bytes=32 time=74ms TTL=56
Reply from 204.44.192.17: bytes=32 time=74ms TTL=56

Ping statistics for 204.44.192.17:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 74ms, Maximum = 76ms, Average = 75ms

mcken@vivo MINGW64 ~/OneDrive/chorus/abv/tech
$ ping sitebuilt.net

Pinging sitebuilt.net [107.175.134.21] with 32 bytes of data:
Reply from 107.175.134.21: bytes=32 time=16ms TTL=56
Reply from 107.175.134.21: bytes=32 time=17ms TTL=56
Reply from 107.175.134.21: bytes=32 time=20ms TTL=56
Reply from 107.175.134.21: bytes=32 time=17ms TTL=56

Ping statistics for 107.175.134.21:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 16ms, Maximum = 20ms, Average = 17ms

mcken@vivo MINGW64 ~/OneDrive/chorus/abv/tech
$ ping parleyvale.com

Pinging parleyvale.com [198.23.148.137] with 32 bytes of data:
Reply from 198.23.148.137: bytes=32 time=33ms TTL=57
Reply from 198.23.148.137: bytes=32 time=33ms TTL=57
Reply from 198.23.148.137: bytes=32 time=34ms TTL=57
Reply from 198.23.148.137: bytes=32 time=34ms TTL=57

Ping statistics for 198.23.148.137:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 33ms, Maximum = 34ms, Average = 33ms

mcken@vivo MINGW64 ~/OneDrive/chorus/abv/tech
$ nslookup -type=ns abvchorus.org
Server:  Fios_Quantum_Gateway.fios-router.home
Address:  192.168.1.1

Non-authoritative answer:
abvchorus.org   nameserver = ns1.lp.hostpapa.com
abvchorus.org   nameserver = ns2.lp.hostpapa.com

## site summary
abv.chorus.org

    "ip": "204.44.192.17",
    "hostname": "s105.servername.online",
    "city": "Los Angeles",
    "region": "California",
    "country": "US",
    "loc": "34.0522,-118.2437",
    "org": "AS23273 HostPapa",
    "postal": "90012",
hosting service: hostpapa  https://hostpapa.com

domain registrar: Tucows Domains Inc. https://www.tucows.com

network packet average round-trip speeds

    abvchorus.org 74ms
    sitebuilt.net 20ms
    parley.vale.com 18ms

server features

      Shared Host
      CPU cores: 2
      Memory: 1GB
      Storage space: 100 GB
      SSH access: NO
      root access: NO
      OS choices: NO
      nginx: NO
      cost/mo: $6.00 (guess)





    <p>Map: 🗺️</p>
    <p>Location: 📍</p>





