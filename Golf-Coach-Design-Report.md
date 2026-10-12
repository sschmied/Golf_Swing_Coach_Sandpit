# Golf Coach – Design Report – Plain Layperson English
Schmied R&D – Version 2026.10.11 – Last Version Only

What is this app?
Golf Coach is a website that works like an app on your phone. You open it, add a video of your golf swing, and it gives you simple coaching tips. All happens on your phone. Your video never leaves your phone. No one else sees it.

Why did we make it this way?
We wanted it private. Many apps send your video to the internet. Ours does not. We also wanted it to work when you have no phone signal on the golf range. After you open it once, it saves a small helper file so it can work offline. We wanted it useful and fun – it shows ghost of good swing, draws path of club, and can follow ball.

How does it work in simple terms?
1. You add a video. Tap big dotted box or drag video in. Normal videos from iPhone or Android work best.
2. It finds your body. Uses free tool from Google that spots shoulders, elbows, wrists, hips, knees, ankles in each picture.
3. It watches how those points move. From movement it can tell when you are setting up, taking club back, at top, swinging down, hitting ball, finishing.
4. It knows about different clubs. Driver near front foot and wide stance. Wedge in middle. For each club we wrote down what good swing looks like. Compares your swing to that.
5. It gives score out of 10 and simple words what is good and what to work on, with drill to try.
6. What you see: left is your video with helpful drawings – ghost player, yellow line for club path, green lines to line up, dot shows weight. Right is simple stick figure and easy charts.

Ball tracing – following the ball – explained simply:
Golfers wanted to see where ball actually went.
- First it guesses where ball starts. Looks at your feet and where that club normally played.
- Then, after you hit ball, it watches rest of video in very small fast version.
- It looks for tiny white or yellow dot that moves from one picture to next. Knows where your body is, so not confused by shirt or hands.
- Remembers how fast ball was going and which way. Adds little drop because of gravity, and little slowing because of air.
- Draws line that fades away, with glowing dot for ball. If loses ball, you can tap screen to point where ball is. You can tell it if ball white or yellow, and make more or less sensitive.
- Also gives rough guess of how far ball went. Just for comparing swings, not perfect measurement.

Sharing for WhatsApp and social media – made small and simple:
Normal videos from phone are very big. WhatsApp does not like big files and they use lot of data.
We added special share button:
- When tap Share Optimized – WhatsApp, it makes smaller copy. Makes picture smaller (720p – still looks good on phone) and squeezes so file much smaller.
- Tries to save as MP4 first, because WhatsApp likes MP4. If phone cannot do that, saves as WebM instead.
- Keeps your sound.
- Draws coaching drawings onto small video – skeleton, ball trail – and adds small green bar at top with our name.
- Cuts out boring bits before you start and after you finish, so only share swing itself.
- Then tries to open phone's normal sharing menu. You can send straight to WhatsApp, Instagram, or email. If not work, downloads small file and opens WhatsApp with ready-made message including score.
- 10-second video that was 30 MB becomes about 3 to 5 MB – small enough to send easily.

What screen looks like and why:
- At top you see logo – hammer above anvil inside black circle – and company name and motto. Feels like workshop.
- Button to switch between Auto, Desktop and Phone view. Auto just looks at how wide screen is.
- Place where you drop video is big dotted box. Works like button – you can click it, tap it, or press Enter on keyboard. Real file chooser hidden (only 1 pixel big) because it looks ugly.
- Videos not too tall, so you can still see advice below without scrolling lot.

Privacy and safety:
No video ever leaves your device. Only thing that comes from internet is body-finding helper file (about 10 MB). When we make small video to share, make temporary link in phone memory and delete after few seconds. No trackers, no ads, no cookies.

What we fixed:
Before, dotted box sometimes did not work when tapped. Now it is proper button that works on all phones, and you can also drag file onto whole page.

What we might add later:
Finding club head itself, guessing how fast club was moving, saving distances over time to see improvement, or using two cameras at once – one from side and one from behind.
