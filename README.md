# Norton Design System RFCs

Many changes, including bug fixes and documentation improvements can be implemented and reviewed via the normal GitHub pull request workflow in [the main design-system repository](https://github.com/wwnorton/design-system).

The "RFC" (request for comments) process is intended to provide a consistent and controlled path for new features to enter the design system, so that all stakeholders can be confidence about its direction.

> **An RFC document is not documentation**.
> This process is intended to shape an idea into something that's ready to implement, not to document the idea or create a specification.
> Please refer to https://github.com/wwnorton/design-system for design system documentation.

## When to follow this process

You should follow this process if you intend to make "substantial" changes to the [@wwnds/core](https://github.com/wwnorton/design-system/tree/main/packages/core) or the [@wwnds/react](https://github.com/wwnorton/design-system/tree/main/packages/react) packages in [the Norton Design System](https://github.com/wwnorton/design-system).

What constitutes a "substantial" change is evolving based on community norms, but may include the following:

- A new component or a new set of components.
- A new feature that creates new API surface area.
    - An "API surface area" includes but may not be limited to any imports from `@wwnds/react` and any props on a React component, any top-level Sass modules in `@wwnds/core` and any new Sass mixins or variables or design tokens.
- Changing the semantics or behavior of an existing API.
- The removal of features that are already shipped.
- The introduction of new idiomatic usage or conventions, even if they do not include code changes to the design system itself.

Some changes do not require an RFC:

- Additions that strictly improve objective, numerical quality criteria (speedup, better browser support).
- Fixing objectively incorrect behavior.
- Rephrasing, reorganizing, or refactoring.
- Addition or removal of warnings.
- Additions only likely to be _noticed by_ other implementors of the design system, invisible to users of the design system.

If you submit a pull request to implement a new feature without going through the RFC process, it may be closed with a polite request to submit an RFC first.

## What the process is

To get a major feature added to the design system, the feature must first be described, discussed, and then merged in this repository as a markdown file.
Once merged, an RFC is officially "accepted" and may be implemented.

1. Work on your proposal in a Markdown file based on one of the two templates found in this repo.
    - [0000-template-component.md](0000-template-component.md) - use this if you are proposing a new or substantially changed component.
    - [0000-template.md](0000-template.md) - use this if your proposal is NOT a new component. This would include new foundational systems, entirely new APIs, or other significant changes to the design system.
    - Put care into the details. **RFCs that do not present convincing motivation, demonstrate understanding of the impact of the design, or are disingenuous about the drawbacks or alternatives tend to be poorly-received**.
2. Open a new pull request. As a pull request, the RFC will receive design feedback from many people, and the author should be prepared to revise it in response.
    - Create your proposal as `accepted/0000-my-feature.md` (where "my-feature" is descriptive. don't assign an RFC number yet).
3. Build consensus and integrate feedback in the discussion thread. RFCs that have broad support are much more likely to make progress than those that don't receive any comments.
4. Eventually, the team will decide whether the RFC is a candidate for inclusion in the design system.
    - RFCs that are candidates for inclusion in the design system will enter a "final comment period" lasting 3 calendar days. The beginning of this period will be signaled with a comment and tag on the RFCs pull request.
    - An RFC can be modified based upon feedback from the team and community. Significant modifications may trigger a new final comment period.
    - An RFC may be rejected by the team after public discussion has settled and comments have been made summarizing the rationale for rejection. A member of the team should then close the RFCs associated pull request.
    - An RFC may be accepted at the close of its final comment period. A team member will merge the RFCs associated pull request, at which point the RFC will become "accepted".
5. Once implemented, a team member will move the RFC to the "implemented" folder.

## The RFC lifecycle

An RFC goes through the following stages:

- **Pending:** when the RFC is submitted as a PR.
    - Pending RFCs can be found in [open PRs](https://github.com/wwnorton/design-system-rfcs/pulls?q=is%3Apr+is%3Aopen).
- **Accepted:** when an RFC PR is merged and undergoing implementation.
    - Accepted RFCs can be found in the [accepted](accepted) directory.
- **Implemented:** when an RFC's proposed changes are shipped in an actual release.
    - Implemented RFCs can be found in the [implemented](implemented) directory.
- **Rejected:** when an RFC PR is closed without being merged.
    - Rejected RFCs can be found in [closed, unmerged PRs](https://github.com/wwnorton/design-system-rfcs/pulls?q=is%3Apr+is%3Aclosed+state%3Aunmerged).

## Implementing an RFC

The author of an RFC is not obligated to implement it.
Of course, the RFC author (like any other developer) is welcome to post an implementation for review after the RFC has been accepted.

An accepted RFC should have the link to the implementation PR listed if there is one.
Feedback to the actual implementation should be conducted in the implementation PR instead of the original RFC PR.

If you are interested in working on the implementation for an "accepted" RFC but cannot determine if someone else is already working on it, feel free to ask (e.g. by leaving a comment on the associated issue).

## Reviewing RFCs

Members of the Norton Design System team will attempt to review some set of open RFC pull requests on a regular basis.
If a team member believes an RFC PR is ready to be accepted, they can approve the PR using GitHub's review feature to signal their approval of the RFC.

**The Norton Design System RFC process owes its inspiration to the [React RFC process], [Rust RFC process], [Yarn RFC process], and [Vue RFC process]**.

[react rfc process]: https://github.com/reactjs/rfcs
[rust rfc process]: https://github.com/rust-lang/rfcs
[yarn rfc process]: https://github.com/yarnpkg/rfcs
[vue rfc process]: https://github.com/vuejs/rfcs