### Task

The Nautilus DevOps team is focusing on improving their data security by using AWS KMS. Your task is to create a KMS key and manage the encryption and decryption of a pre-existing sensitive file using the KMS key.

Specific Requirements:

1. Create a symmetric KMS key named `xfusion-KMS-Key` to manage encryption and decryption.
2. Encrypt the provided `SensitiveData.txt` file (located in /root/), base64 encode the ciphertext, and save the encrypted version as `EncryptedData.bin` in the `/root/` directory.
3. Try to decrypt the same and verify that the decrypted data matches the original file.
   Make sure that the KMS key is correctly configured. The validation script will test your configuration by decrypting the EncryptedData.bin file using the KMS key you created.

### Solution

- Create key

  ```bash
  aws kms create-key --description "xfusion-KMS-Key"
  ```

- Create the alias using the key-id got from the output of the above command

  ```
  aws kms create-alias --alias-name alias/xfusion-KMS-Key --target-key-id <key-id>
  ```

- Encrypt the file

  ```
  aws kms encrypt \
   --key-id alias/xfusion-KMS-Key \
   --plaintext fileb:///root/SensitiveData.txt \
   --output text \
   --query CiphertextBlob | base64 --decode > /root/EncryptedData.bin
  ```

  `--query` part extracts the `CiphertextBlob` field and converts the base64-encoded data to raw binary.

- Decrypt and get the diff

  ```bash
  aws kms decrypt \
   --ciphertext-blob fileb:////root/EncryptedData.bin \
   --output text \
   --query Plaintext | base64 --decode > /root/DecryptedData.txt

  diff /root/SensitiveData.txt /root/DecryptedData.txt
  ```

  The `diff` output should be empty if content of the both files are same.
