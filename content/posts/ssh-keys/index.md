---
title: "🔑 SSH Keys"
date: 2023-06-28
---

## Generating keys

On your client computer run ``ssh-keygen``

It'll ask where you want to save the file.
Just hit enter to use the default .ssh directory.

It'll also ask for a passphrase to the ssh key.
Just hit enter to not use a passphrase.
Hit enter once again when it asks for the same passphrase.

```
+---[RSA 3072]----+
|                 |
|                 |
|          .      |
|         o .     |
|        S   *    |
|         . O.*.  |
|       .o+=oB++  |
|       +@+=*+o=+.|
|      .=B%* oE+oo|
+----[SHA256]-----+
```

Once your key is generated, it'll output a randomart image for the key.

## Transfering keys

Run ``ssh-copy-id user@remote-host``
It'll ask you to input the password for the user.

Now you should be able to ssh into the server without putting in the password.
