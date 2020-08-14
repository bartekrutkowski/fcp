---
authors: Bartlomiej Rutkowski <robak@FreeBSD.org>
state: draft
---

# FCP-20200606-hardening: Changing FreeBSD Security Defaults

Change the default settings of various FreeBSD security features in time
for FreeBSD 13.0-RELEASE.

## Problem Statement

FreeBSD comes with many security features available but the default install
nor the VM images provided are not using them resulting in rather medicore
security level of default FreeBSD installations, lagging behind the market
trends and competing projects.

## Problem Discussion

Over the years FreeBSD has introduced new security features.  Most of them
was left disabled by default, and was not exposed nor advertised to the users
in any significant manner, making their discovery and usage harder for less
experienced or new users unless they knew what they wanted and what they were
looking for.

With every major exploit happening (list: shellshock, bleeds, etc) the demand
for better security defaults increases, and the market expectations are moving
towards these operating systems that are more proactive towards their security.

At the same time, the typical use case of Unix/Linux systems changed
significantly, with multi-user systems being much less common than it used to
be in past and systems delivering one service, architecture promoted by the
microservices paradigm, away from big, multi-service, multi-process monolithic
systems.

This increased market interest in security, and change in how operating systems
are being used makes the main argument used in past for not enabling these
available features by default due to users convenience not valid anymore.

Another major argument in past was relative new-ness some of these features,
and uncertainty with regards to their safety and stability due to lack of
extensive testing and usage.

That as well should not be a valid point anymore, as most of these features
are now years old and widely used, with no negative reports or major stability
issues or significant performance regressions.

For example:

```
security.bsd.see_other_uids=1
security.bsd.see_other_gids=1
```

The above kernel defaults are available since 5.0-RELEASE, and are completely
safe to use, stable and not making much sense in todays non user focused,
microservice oriented system deployments.

In addition to the above, lagging behind the trends in security exposees
the FreeBSD Project to a lot of bad press and resonates strongly in the open
source community discouraging potential new users from even trying FreeBSD.

## Proposed Solution

The proposal is to change the following default system settings in all relevant
places (eg. config files, bsdinstall scritpts, VM and cloud images, etc.):

```
security.bsd.see_other_uids=0
security.bsd.see_other_gids=0
security.bsd.see_jail_proc=0
security.bsd.unprivileged_read_msgbuf=0
security.bsd.unprivileged_proc_debug=0
kern.randompid=(to be randomly generated)
machdep.hyperthreading_allowed=0
kern.elf32.aslr.pie_enable=1
kern.elf32.aslr.enable=1
kern.elf64.aslr.pie_enable=1
kern.elf64.aslr.enable=1
vm.cluster_anon=2
net.inet.ip.random_id=1
```

## Solution Discussion

The plan is to make this changes ready for 13.0-RELEASE but to make it so that
the community and users would be informed about the change well in advance
and to not violate the POLA.

Following are the proposed steps of implementation:

0. Acceptance of this FCP.
1. Announcement on the relevant mailing lists (at least on current@, hackers@,
   and security@).
2. Commits landing in CURRENT before branching stable/13, no MFC is planned.
3. Announcement on FreeBSD Twitter accounts.
4. Announcement/article on the FreeBSD website.
5. Announcement on remaining channels (Reddit, FreeSBD forums, etc.).
6. FreeBSD 13.0-RELEASE with new defaults.
7. Turn off (in HEAD) sysctls that interfere with debugging after stable/13
   is branched (TBD).

## Final Disposition

This section is for detailing the reasoning behind the final status of the FCP.
If you withdraw your FCP you can outline your reasons here. If core accepts
or rejects the FCP they can add their reasoning here. It should not be present
in FCPs prior to the `accepted`, `rejected` or `withdrawn` states.
