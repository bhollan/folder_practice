**TL; DR: If your file's path starts with `/` or a drive letter like `C:/`, then it's absolute and you'll lose points for it. If it starts with `./` or `../`, you're probably fine.**

If that makes total sense to you, you're done. If none of that makes any sense, read on.

# This is absolutely relative


Some of you keep losing points for using absolute paths vs relative paths in your problem set code. Using absolute paths is not a problem when you're running code on your own local machine. However, it will 100%, inescapably, always, be a problem when running that same code in almost any other circumstance. Let me illustrate.

I have a CSV file on my machine here:
```
/home/bhollan/code/game_acquire_stats/players.csv
```

If you had downloaded that same file, it might be here, for example:
```
/Users/gracieLou/downloads/players.csv
```

Or for windows users after they moved the file to somewhere new:
```
C://Windows/Apps/Jupyter/Whatever/myAwesomeProject/players.csv
```

What's the ONLY important part of those? Right! `players.csv`.

Now imagine you've written something awesome. And you have posted loads of stuff inside a repository. Something like this:
```
  CoolStoryBro/
    raw_data/
      games.csv
      tournaments.csv
    clean_data/
      players.csv
      rankings.csv
      awards.csv
    scripts/
      cleaning_tournaments.ipynb
      scraping_scores.py
      modelling.ipynb
      dashboard.ipynb
    figures/
      playtime.png
      distribution.jpg
      logo.bmp
      layout.tif
    archive/
      whatever.garbage
    .git/
```


So let's say inside ALL those /scripts/ you used absolute paths. Then let's say that either:

```
A: you run your code on a server
B: a friend/co-worker/whoever wants to run your code on their machine
C: a TA wants to run your code to check your work ;)
D: an employer wants to run your project to witness how awesome you are
E: you find your own code after 2 years and you have a different computer now
F: you want to show a friend your cool project, but you don't have your computer
G: you copy the whole folder's project into your /projects/ folder from where it used to be
```

In ALL, not some, not most, but _ALL_ of those scenarios, NONE of your scripts will run now. They will ALL be broken and need maintenance.

So if you have a line of code in your script like:
```
plt.scatter(x, y, marker='+').savefig('/Users/gracieLou/code/CoolStoryBro/figures/playtime.png')
```

This will now either trow an error, or worse, just simply write to the old folder that you're not using anymore that just happens to still be there. You'd run that line of code thinking the file would be in the 'new' location, but it would still be writing it to the old one! It wouldn't even tell you! So rude, right?!

The reason is that it's an absolute path. It starts at the absolute root directory and navigates from the very beginning to the very end in absolute, non-negotiable terms.

A better way (there are indeed many better ways, not exactly only one 'right' way) would be to write it something like this:
```
plt.scatter(x, y, marker='+').savefig('../figures/playtime.png')
```

So this is code that's running inside the `scripts` folder, so the `..` goes UP ONE LAYER/DIRECTORY in the tree, navigates DOWN into the `figures` folder, and writes the `playtime.png` file in there. This makes your code more readable, more robust, and more transferrable and would now not cause any problems in any of scenarios A-G as described above.

If you need to use absolute paths during development/playing/experiments, GO FOR IT! But anything that I grade will get points taken off for having absolute paths in it. It's bad practice in every respect and has no 'upside' to it.

The way I write my paths is using the pandas shortcuts (`<grumpy-old-man-voice>`"They didn't even have those when I was your age, see!"`</grumpy-old-man-voice>`).  It's quite simple. Just start typing something like:
```
pd.read_csv('./')
```
Then with your cursor inside the `('')`'s, right after the `./` just hit the `<TAB>` key and it will open up a drop down that you can navigate. If you don't see your file, but you see the folder your file is in, do not dismay! Just select, for example, the `raw_data` folder from the drop-down and press `<ENTER>` and then `<TAB>` again.


But Brian! I used:

A: pwd

B: os.chdir

C: root = pwd();  os.chdir(root)

Well, os.chdir will just change the current directory to whatever you give it (which is WAY WORSE). So if you give it an aboslute path, it doesn't fix the problem, it actually makes many, many more. And C is the same thing as doing sqrt(x**2) or "the squaure root of x squared" which is just x. The handiest shortcut for "whatever-folder-I'm-in-now" is just `.`.  Literally a dot. And the shortcut for "the-folder-immediately-up-one-layer" is `..`. So if you're in the `scripts` folder and you want to load the players.csv file, you would do:
```
../clean_data/players.csv
```

Or for the games file:

```
../raw_data/games.csv
```

So I hope this has made things clearer. If not, please feel free to WhatsApp, email, officeHours me about this. It's actively fun for me to talk about this stuff because I've been doing it for so long (literally since the 90s).

PS:
Having spaces in folder and file names is equally as disgusting to me. But it won't objectively break things as much as having an absolute path in your code. So I don't take points off of it even though it makes me cringe.
PPS:
Below is a list of reading/watching resources about the difference if anything I said was unclear. 
PPPS:
Below THAT is some exercises that ChatGPT and I came up with to play around with navigating to/through/from files in directory structures. I promise this is not a useless skill.



LINKS:
------------------
https://www.linkedin.com/pulse/difference-between-absolute-path-relative-linux-waqas-muazam/
https://www.redhat.com/sysadmin/linux-path-absolute-relative
https://stackoverflow.com/questions/21306512/difference-between-relative-path-and-absolute-path-in-javascript
https://teamtreehouse.com/community/difference-between-absolute-path-and-root-relative-dont-they-both-start-from-the-root
https://medium.com/@tamerberatcelik/relative-paths-vs-absolute-paths-understanding-the-basics-of-file-navigation-5332de0550b6
https://www.youtube.com/watch?v=ephId3mYu9o&ab_channel=Udacity


EXERCISES
------------------
https://chat.openai.com/share/e78cf29c-5499-4fea-8f12-848c4314a5d3


This has a lot of commands that have already been run in this repo.
If you fork and/or clone it, you should be able to play around with the files/folders.
The image below is of how the folder structure results from the commands specified at the URL above (and was made in the command line with the `tree` package).
Just clone this repo if you want to play with them. You can do no harm.



![image](https://github.com/bhollan/folder_practice/assets/8443608/b285fbfa-ca7d-434f-ae2b-713548bec9de)
