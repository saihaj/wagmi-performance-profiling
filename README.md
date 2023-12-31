# Performance profiling

Trying to investigate initial load impacts of `wagmi`.
Before the browser can paint anything to the page, `wagmi` context seems to be doing something expensive that it takes blocking time on the main thread.

After some investigation, it seems like most of the compute is being spent on some ENS normalizations.
![ENS normalize biggest culprit](screenshots/1.png)

You can check the [performance profile](./wagmi-load.json.zip) and investigate other things.

This is concerning because we are just showing a connect button and there is no wallet connected and it seems to be crunching some time on ENS.

In an ideal world, to optimize speed we should be able to defer loading of most of things from `wagmi` until user interacts with the connect button.

### To reproduce
1. Clone this repo
2. `pnpm install` - install dependencies
3. `pnpm dev` - start dev server
4. Open `localhost:3000` in Chrome
5. Open devtools and go to `Performance` tab
6. Click on start profiling and reload the page (on Mac Cmd + Shift + E)
7. Take profile for 3 seconds
