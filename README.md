# my-yt-neuter

A personal, lighter cut of [mchangrh/yt-neuter](https://github.com/mchangrh/yt-neuter): hand-picked uBlock Origin rules that make YouTube faster and quieter without breaking the parts I use.

Every rule is tagged with a short code (`[Y32]`, `[M7]`, ...) that points back to the yt-neuter list it came from.

-----

## Install

[add to uBo](https://subscribe.adblockplus.org/?location=https%3A%2F%2Fraw.githubusercontent.com%2Fdotd1%2Fmy-yt-neuter%2Fmain%2Fmy-yt-neuter.txt&title=My%20YouTube%20Neuter)  

Filter URL  
GitHub: `https://raw.githubusercontent.com/dotd1/my-yt-neuter/main/my-yt-neuter.txt`  

Or paste the contents of [`my-yt-neuter.txt`](./my-yt-neuter.txt) into uBlock Origin → Dashboard → **My filters**.

## What it does

### Speed (blocked downloads)
 - No hover previews on thumbnails (`inline_preview.js`)
 - No Lottie animation engine or animation files (like / subscribe animations)
 - No Chromecast / phone-remote code (removes the Cast button)
 - No rolling view / like counters and no live metadata refresh
 - No end-screen module, no offline-download module
 - No doodle logos, no comment emoji-picker data, no "install as app" manifest
 - No profile pictures, channel banners or channel emoji (`yt3.ggpht.com`, `yt3.googleusercontent.com`)

### Home page
 - All shelves removed (Shorts, games, Playables, Breaking news, free movies, posts): just the video grid
 - No category chips, no members-only videos, no "Free with Ads" movies

### Shorts
 - Shorts shelves, sidebar entry, channel tab and Shorts-icon cards hidden

### Watch page
 - Ambient mode, seek feedback, hashtags, "Trending" super-title hidden
 - Join / free trial / merch / tickets / products / fact-check boxes hidden
 - Sponsored videos, ads panel, shopping panel, fundraisers, movie offers hidden
 - Live chat: no Super Chat ticker, paid messages, memberships or emoji-only messages

### Everywhere
 - Surveys, Premium bitrate nags, "Become a member" pop-ups
 - Voice search, country code, sign-in nag, Premium link, sidebar footer
 - "New", "4K", "Fundraiser" badges, Mix and YouTube Music playlists in recommendations

## Optional: hide videos under 1K views

This one is a separate list, so it stays off unless you add it. It hides any video card with fewer than 1,000 views, which clears out the low-view videos YouTube pushes into Home, the sidebar and search.

[add to uBo](https://subscribe.adblockplus.org/?location=https%3A%2F%2Fraw.githubusercontent.com%2Fdotd1%2Fmy-yt-neuter%2Fmain%2Fhide-under-1k-views.txt&title=My%20YouTube%20Neuter%3A%20under%201K%20views)  

Filter URL  
GitHub: `https://raw.githubusercontent.com/dotd1/my-yt-neuter/main/hide-under-1k-views.txt`  

Or paste the contents of [`hide-under-1k-views.txt`](./hide-under-1k-views.txt) into **My filters**.

### How the counter works
YouTube shortens view counts once they reach 1,000:

| Real views | Shown as | Hidden? |
|---|---|---|
| 0 | `No views` | **yes** |
| 7 | `7` | **yes** |
| 523 | `523 views` | **yes** |
| 999 | `999` | **yes** |
| 1,000 | `1K` | no |
| 1,250,000 | `1.2M views` | no |

So **any count under 1,000 is a plain number of 1 to 3 digits** (or "No" for zero), sometimes with the word "views" after it. Each rule finds the view-count text on a card and tests it with the regex `/^(\d{1,3}|No)( views?)?$/`:
 - `^` and `$`: the whole text must match, nothing before or after
 - `(\d{1,3}|No)`: one to three digits, or the word "No"
 - `( views?)?`: optionally followed by " view" or " views"

`999`, `523 views` and `No views` pass, while `1K` or `1.2M views` fail because of the letter after the digits. When the text passes, `:has()` hides the whole card that contains it.

> **English only:** "views" and "No" are matched in English. With YouTube set to another language, only bare numbers (`523`) are caught.

Where it looks for the count:
 - **New layout** (Home, sidebar, subscriptions): the text right after the eye icon (`.ytContentMetadataViewModelLeadingIcon + .ytContentMetadataViewModelMetadataText`)
 - **Old layout** (search results): the first metadata item under the title (`.inline-metadata-item:first-of-type`)

## What it breaks
 - **Profile pictures** are blank everywhere, including your own avatar
 - **Cast button** is gone
 - **Emoji** in comments and chat are blank; emoji-only chat messages are hidden
 - **Live viewer count** on streams stops updating
 - **Download** button and Premium offline downloads
 - **Hashtag links** everywhere

If videos sometimes do not start after clicking a thumbnail, disable the `scheduler.js` line under `[Y32]` first: it runs YouTube's queued background jobs.

## Credits
All rules come from [mchangrh/yt-neuter](https://github.com/mchangrh/yt-neuter) (Unlicense). Changes from the originals are listed at the top of the filter file.
