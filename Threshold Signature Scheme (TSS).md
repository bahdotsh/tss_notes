---
title: Threshold Signature Scheme (TSS)
created: '2022-02-08T05:43:57.545Z'
modified: '2022-02-08T06:53:34.308Z'
---

# Threshold Signature Scheme (TSS) 

- Threshold Signature Scheme (TSS) is a cryptographic protocol for distributed key generation and signing. TSS allows constructing a signature that is distributed among different parties (for example three users), and each user receives a share of the private signing key

- Init 

`tss init` will create home directory of a new tss setup, generate p2p key pair.
```
./tss init --home ~/.test1 --vault_name "default" --moniker "test1" --password "123456789"
./tss init --home ~/.test2 --vault_name "default" --moniker "test2" --password "123456789"
./tss init --home ~/.test3 --vault_name "default" --moniker "test3" --password "123456789"
```

- Channel 
`tss channel` will generate a channel ID for bootstrapping. One party can generate a channel, then share the generated channel ID with others offline.

```
./tss channel --channel_expire 30
```

- Keygen 
This command will generate the private key and share the secret. Everyone needs to agree on the password of this private key. The password length must be larger than eight.

```
./tss keygen --home ~/.test1 --vault_name "default" --parties 3 --threshold 1 --password "123456789" --channel_password "123456789" --channel_id ""
./tss keygen --home ~/.test2 --vault_name "default" --parties 3 --threshold 1 --password "1234567890" --channel_password "123456789" --channel_id ""
./tss keygen --home ~/.test3 --vault_name "default1" --parties 3 --threshold 1 --password "1234567891" --channel_password "123456789" --channel_id ""
```

- Sign
```
./tss sign --home ~/.test1 --vault_name "default" --password "123456789" --channel_password "123456789" --channel_id ""
./tss sign --home ~/.test2 --vault_name "default" --password "123456789" --channel_password "123456789" --channel_id ""
```

- Regroup

This command will generate new_n secret from the same private key, and it will be shared with new_t threshold. At least old_t + 1 should participante in signing

```
# start 2 old parties (answer Y for isOld and IsNew interactive questions)
./tss regroup --home ~/.test1 --vault_name "default" --password "123456789" --new_parties 3 --new_threshold 1 --channel_password "123456789" --channel_id ""
./tss regroup --home ~/.test2 --vault_name "default" --password "1234567890" --new_parties 3 --new_threshold 1 --channel_password "123456789" --channel_id ""
# start the new parties (answer n for isIold and Y for IsNew interactive questions)
./tss regroup --home ~/.test3 --vault_name "default" --password "123456789" --new_parties 3 --new_threshold 1 --channel_password "123456789" --channel_id ""
```
