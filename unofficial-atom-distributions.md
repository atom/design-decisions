# Unofficial Atom Distributions

**Question:** Some Linux enthusiasts are distributing unofficial customized versions of Atom with mismatched components that neither the Atom Core team nor GitHub have tested, certified or authorized. Do we support these unofficial customized versions of Atom? Can we support these unofficial versions of Atom?

## Why is Customizing Atom a Problem?

Often these customized versions of Atom are modified versions of the Stable branch of Atom with mismatched versions of built-in components. These updated components are taken from the Beta or `master` branches and then injected into the Stable branch and a new Atom package is generated.

The way Atom is designed and built by the Atom Core team, built-in components are tested and verified to work with specific versions of all the other built-in components. Importing mismatching versions of built-in components often lead to uncaught exceptions and crashes because of mismatched or even missing APIs. These exceptions and crashes can cause a lot of effort to track down and the solution is often to simply install the properly tested version.

## Support

The Atom Core team and GitHub currently support three released versions of Atom (`master`, Beta and Stable) across three major platforms (Linux, macOS and Windows). This generates hundreds of new Issues a week, not to mention the over 4,000 open Issues that we already have. Since these unofficial customized versions of Atom have started turning up, we have noticed an uptick in new Issues being reported.

## Decision

The bottom line is, we don't have the resources to support these unofficial versions of Atom. Atom has plenty of valid bugs and enhancements that we want to make progress on. Every one of these Issues that we have to investigate takes time away from making Atom better.

Therefore, any Issue reported from an installation of Atom that is discovered to contain mismatched components will be closed without further investigation.

## FAQ

**Q:** Does this mean that we can't experiment? I thought Atom was open source?

Atom is open source. We want people to experiment with it. We want people to try new things. Go nuts! We just don't have the resources to support your experiments the way we support the versions of Atom we release.
