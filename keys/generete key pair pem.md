# Steps to Create an SSH Key Pair in PEM Format on Linux

## 1. Open the terminal

## 2. Generate the SSH key pair
Run the following command to generate the SSH key pair using RSA with 4096 bits. This will create a `.pem` file:

```bash
ssh-keygen -t rsa -b 4096 -m PEM -f ~/key.pem
```

- `-t rsa`: Specifies the key type (RSA).
- `-b 4096`: Specifies the key size (4096 bits).
- `-m PEM`: Outputs the key in PEM format.
- `-f ~/key.pem`: Specifies the filename and location for the key (in this case, it will be saved in the home directory as `key.pem`).

## 3. Set permissions for the private key
Make sure the private key has the correct permissions so it’s secure:

```bash
chmod 400 ~/key.pem
```

## 4. (Optional) Add the key to the SSH agent
If you want to use the key without specifying it every time, you can add it to your SSH agent:

```bash
eval $(ssh-agent -s)
ssh-add ~/key.pem
```

## 5. Check for the public key
The public key will be saved in a file called `key.pem.pub`. You can view it with:

```bash
cat ~/key.pem.pub
```

Now, your SSH key pair is ready to use!
```

