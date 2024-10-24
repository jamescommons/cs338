# Cookies and cross-site scripting (XSS)

Author: James Commons

Date: October 24, 2024

## Part 1: cookies

(a)

There are three cookies. One is called remember_token, and it has a value of
"5|af5c0cc2156a1ab3d69d7b853575f1ec0a4c138483fb9afe3d7156daa55f5828a1261bc5c258674f684f1ad64c4165140d25b09750a3dc322bd994874b5a4959".
This cookie seems to be used for keeping me logged in as Eve even after I close
the session. I assume it is some sort of random number that only I could know
and serves as a sort of password that gives me access to the website as Eve.

The second cookie is called session. It has a value of 
".eJwlzjsOwjAMANC7ZGaI41_cy1RJbAvWlk6Iu1OJAzzpfcqeR5zPsr2PKx5lf3nZCgL1yoM8RnecXG24pTp3IEKeIaTNhk2p3qgBj6kLrQVE6lzkXTqs6GlWaTYGltqGL0TJG8mIRK09HJZJomCr4YqJFGyqWO7Idcbx33D5_gCMbC6Z.ZxqiJw.b8q1W1GZd80KI0Kqro-RE77DCpo".
This cookie expires at the end of the session, and I assume it is what keeps me
logged in when I navigate between pages on FDF.

The final cookie is called theme, and its value is default. This one is a lot
more straight forward. It just tells the server that I want my theme to be the
default theme before it sends the HTML for the page.

(b)

The theme cookie changed its value to "blue", which is the same as the theme
I selected. The other cookies stayed the same.

(c)

The first thing the server responds with when I request GET /fdf/ is a 200 OK
with a Set-Cookie header with the value theme=default. This sets my theme
immediately to be default. The next time I try to visit the website, the server
does not have to include that header again because it already knows my theme
preference (because I send it in the form of a cookie every time I make
a request).
When I try to change the theme, the first thing my computer does is send
a GET request to the server with a URL parameter theme=blue. In this request,
it still sent the cookie as theme=default because the server has not responded
telling my browser to change the cookie yet. That happens in the response,
which contains the header Set-Cookie: theme=blue (+some expiration date), along
with the HTML for the blue website. Now, every subsequent request on my end
comes with the header Cookie: theme=blue.

(d)

 It is still blue!

(e)

 Everytime I send a request, the Cookie header is populated with whatever
stateful data the server wanted me to store locally (like theme preferences).

(f)

It is first transmitted as a URL parameter in the GET request for the homepage,
and then it is stored as a cookie.

(g)

I went into the browser inspector's application menu, selected cookies, found
the theme cookie, and manually changed the value by typing red. After reloading
the page, the theme was updated (the page turned red).

(h)

Before sending the packet off to the server, I went into the Cookie header and
manually changed the theme cookie back to blue. The server sent a Set-Cookie
header back that said theme=blue (I assume because it previously had a record
of my cookie being red, and it wants to keep everything up to date).

(i)

I found the file. It's called ~/.BurpSuite/pre-wired-browser/Default/Cookies.
It's a SQLite database. I was even able to find the exact theme cookie we have
been talking about using a SQL query (although the binary data was not readable
in the terminal).

## Part 2: cross-site scripting (XSS)

(a)

1. The first thing Prof. Moriarty does is post the attack on the discussion forum.
The post contains an HTML escape sequence that it left unsanitized, will be
interpreted by the browser as real HTML commands. 

1. When a user clicks on Prof. Moriarty's post, the browser does not display
the malicious HTML code, and instead it renders it as if it were source code.
This can happen way after the inital post was created. This means that this
attack is not real time, and can sit dormant without constant monitoring from
Prof. Moriarty.

1. Next, the rendered output is displayed (or javascript is executed for the
second attack) on the victim's computer, completing the attack. This could have
been a lot worse if the attack were something more complicated than turning
text red.

(b)

With JavaScript, it is possible to download cookies using document.cookie. You
could use javascript to send something like a POST request to a third party
server you control and steal the cookie data, which lets you log in as that
user if they have the remember setting turned on for example.

(c)

You could also redirect the user to a fake site that immitates FDF's login
screen. It could pop up with a message like "oops, something went wrong" and
prompt you to log in again. Only, you would not actually be logging into the
correct website, and I would again be able to steal your credentials.

(d)

One super important thing that the server can do to prevent this attack is
sanitize all user input. This means that the user should either remove or
escape things like HTML sequences or SQL query words depending on the context.
Anything a user types into the website should be treated strictly as text, and
absolutely nothing else. The browser can also try to prevent XSS by looking for
HTML or JS code that looks like it does XSS things. I think this is called
a content security policy.

