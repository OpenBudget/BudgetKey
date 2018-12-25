We use a pretty standard workflow, based on GitHub.

If you've never worked with `git`, it's best to do a short intro to `git` - [This one](https://try.github.io/), for example.

In case you've never worked with GitHub, it's best to do a short intro to that as well - for example, GitHub's own [training](https://services.github.com/on-demand/intro-to-github/)

What you would basically do, after being assigned a task would be to:
- [ ] Make sure you're assigned to the issue.
  - You can assign yourself by simply leaving a comment on the issue saying "I'm working on this :)".
  - You will sometimes need to be added to the repo. Ask Adam about it (don't forget to accept the invitation [here](https://github.com/OpenBudget)).
- [ ] Make sure you've read the relevant tutorials and HowTo-s (see the main [README](../README.md) for links)
- [ ] Find out which repo you need to modify, and Fork it using GitHub's UI (use the 'fork', Luke!)
- [ ] Clone your forked repo using the `git` (`ssh`) protocol (not the `https` one).
   - This would be a good time to set up your `ssh` keys with GitHub, if you've never done so.
- [ ] It's best at this point to create a new branch for this issue, with a good name (e.g. `add-support-for-butterflies`)
- [ ] Make the requested change. Test it, document it, clean up your code :) 
- [ ] You can now commit your code and push it.
- [ ] Go to GitHub, and open from there a Pull Request. Make sure to review the code in the Pull Request to make sure that it includes all you intended to change (and nothing more)
- [ ] Talk to your repo's master (or Adam) to see that your change gets merged.
- [ ] After merging, take a look at the production website to see that everything works as intended.

