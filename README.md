# ndn-security-discussions
This repo documents some general NDN security issues that don't have implementation plans in short term. Feel free to pull request if you have good ideas.
Some of them come from the conversations with my advisor Prof. Lixia Zhang, and email exchanges with other NDN team members.

## Signing
1. The current keylocator only identifies the signing identity. 
Therefore, the keylocator causes ambiguities about the signing keys. 
Upon validator's requests, the network can return a different certificate other than the actual signing certificate.
Junxiao has reported this issue years back and proposed a solution in his NDNts (can't remember the details).
The NDN team agrees to put complete signing certificate name into the keylocator in future.
Regarding this point, we need revise Yingdi's 2015 trust schema paper.

## Certificate
### NDNCERT
1. NDNCERT v0.3, as the latest certificate issuance protocol, reflects outdated boostrapping understandings.
The naming process should have nothing to do with a certificate issuance protocol.
One needs to obtain its name first before requesting the certficate.
However, the optional PROBE step can easily cause confusions on the client side, so that client may fall back on obtaining naming assignment from the CA.
Despite the minimal implementation for testbed, we need evolve the NDNCERT design.

### Availablity
1. There'a always an issue: when the validator requests a certificate, where it is?
As a special application, NDN Testbed uses repo-ng to serve all certificates under its trust zone.
However, small applicatons need their own way of serving signing certificates.
Alex pointed out this is an issue we keep revisting at.
In early days, we thought about making PIB available from the network, but somehow that idea went no where.

## Testbed
1. In reality, NDN Testbed only accepts one trust anchor with one trust policies: testbed hierachical policy.
This limits any application who wish to utilize testbed as NDN connectivity can only be certified by testbed first.
This not only adds complexity to the application, but also violates the "local trust anchor" principle in NDN.
Back in 2017, openmhealth became accepted as a second trust anchor, so openmehealth data producer no longer needed a testbed certificate to sign routing announcement.
In 2022, the NDN team is considering about adding any another trust anchor to the testbed.
So far, adding a new trust anchor to testbed heavily replies on out-of-band authorization from the NDN team.
In future, this process could be simplified by letting the testbed root certificate endorses the new trust anchor (not fully discussed yet).

