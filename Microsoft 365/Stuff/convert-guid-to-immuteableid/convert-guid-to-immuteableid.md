---
title: Convert GUID to ImmuteableId
parent: "General stuff"
grand_parent: "Microsoft 365"
tags: microsoft 365
---

# Convert GUID to ImmuteableId
And vice versa...

## GUID -> ImmuteableId
```
[Convert]::ToBase64String([guid]::New("87475e6b-319f-431e-a283-cbba08afb1ee").ToByteArray())
```


## ImmutableId -> GUID
```
[Guid]([Convert]::FromBase64String("a15Hh58xHkOig8u6CK+x7g=="))
```