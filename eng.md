#Serving Fonts from CDN

For maximum performance, we all know we must put our assets on CDN.  Along
with those assets are custom web fonts.  Unfortunately custom web fonts via CDN 
don't work in Firefox or Internet Explorer by default -- they require custom 
CORS settings to display properly.  Here's the code you'll need to make that 
happen.

## The `.htaccess` or `httpd.conf` Code

The code can be placed with the `.htaccess` file or `httpd.conf` file:

    <FilesMatch ".(eot|ttf|otf|woff)$">
    	Header set Access-Control-Allow-Origin "*"
    </FilesMatch>
    

This sets the Access-Control-Allow-Origin CORS configuration to allow pulling
from all domains.  List specific domains by comma if you want to serve fonts up 
to only specific domains.  You'll want to serve up all font types appropriately 
in the case that the browser prefers one type.

This same strategy is used by [Bootstrap CDN][1], so you know it's good!</
article
>

 
## Be Heard

Tip: Wrap your code in <pre> tags or link to a GitHub Gist!

[Older 
 Using CSS attr and content for Tooltips ][2] </section> <footer>

© [David Walsh][3]  2007-2013. [Feedback][4] </footer>

 [1]: https://github.com/netdna/bootstrap-cdn/blob/master/.htaccess#L41
 [2]: http://davidwalsh.name/css-attr-content-tooltips
 [3]: http://davidwalsh.name/
 [4]: http://davidwalsh.name/contact