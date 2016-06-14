# Electron Update Procedure

**Question:** How do we decide when to update to a new version of Electron? How do we update Atom from one version of Electron to the next? How can external contributors help?

## Decision Process

We decide to update to a new version of Electron when there is an enhancement or bug fix that version would enable that would benefit Atom users. Because updating to a new version of Electron is non-trivial, sometimes we skip versions until there is a clear benefit that justifies the cost.

## Change Process

Like most things, we use the standard [GitHub Flow][flow]:

1. Create a branch
1. Add commits
1. Create a Pull Request
1. Discuss and review your code
1. Merge

Since most Electron updates require lots of testing and associated changes, these often require a lot of review, discussion and additional commits. (For example see [the PR to update to Electron v0.37][electron-pr].) We like doing all of this work in one PR because this strategy:

1. Allows us to continue focusing on moving Atom forward without getting bogged down by Electron changes
1. Allows us to focus the people who are interested in having the latest Electron in one place
1. Allows us to make sweeping changes when they are necessary without having to nickel-and-dime ourselves with one- or two-line PRs *ad infinitum*
1. Allows us to ensure that packages remain compatible with the older version of Electron even as they are updated
1. Allows us to easily abandon Electron changes when it makes sense (such as to skip Electron versions)

## Contributing

If there is a version of Electron that you want Atom to be using that it isn't yet, you may [open a Pull Request][pr] and start the process. Please [do a search first][electron-pr-search] to ensure that there isn't one already open for that version or an earlier version. If there is a PR already open for an earlier version than the one you want, it is better to help the earlier version get delivered than to start a new PR. Additionally, if you open an Electron update PR please state specifically what benefit updating will provide to Atom users. Maybe there is a benefit that the Atom team has overlooked!

## FAQ

**Q:** Should I open an Issue if Atom is not on the latest version of Electron?

No. Updating to the latest version of Electron is an ongoing task that we will do as and when we can.

**Q:** Why can't you stay closer to the latest version?

Updating to new versions of Electron, even patch releases, is almost always non-trivial. Since Electron contains the base libraries that form the underpinnings of everything in Atom, even small changes in Electron can create significant problems. Updating to the latest version of Electron is something we undertake only when the benefits outweigh the cost. If there isn't a specific benefit to Atom's users, we won't upgrade to that version of Electron.

**Q:** Where can I ask more questions?

Please open a new topic on [Discuss, the official Atom and Electron message board][discuss].

[discuss]: https://discuss.atom.io
[electron-pr]: https://github.com/atom/atom/pull/11474
[electron-pr-search]: https://github.com/atom/atom/pulls?utf8=%E2%9C%93&q=is%3Apr+is%3Aopen+electron
[flow]: https://guides.github.com/introduction/flow/index.html
[pr]: https://help.github.com/articles/creating-a-pull-request/
