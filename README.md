# ZTySecurityCipher Class Documentation

## Overview

`ZTySecurityCipher` is a class designed for ultra-secure encryption and decryption using the AES algorithm in GCM mode with no padding. It combines advanced techniques such as PBKDF2 key derivation, UUID handling as AAD (Additional Authenticated Data), and fully random generation of salts and initialization vectors (IVs).

Its main goal is to make life miserable for potential attackers, ensuring both the confidentiality and authenticity of encrypted data.



## Key Features

- **Strict UUID Validation**: Ensures the UUID is in the correct format before proceeding.
- **Random Salt and IV Generation**: No reuse of data or mixing with the UUID.
- **UUID as AAD**: Adds an extra layer of security during encryption and decryption.
- **Modular Task Separation**: Clean and maintainable code with dedicated methods for key derivation, validation, data combination, and extraction.
- **Custom Exception Handling**: Differentiates between authentication errors and general operation errors for easier debugging.



## Prerequisites

- Java 8 or higher
- Android API Level 23 or higher (for full AES/GCM support)



## Class Initialization

`ZTySecurityCipher` is a fully static utility class. No instantiation is required; simply import the class and call its methods directly.

```java
import com.security.cipher.zty.ZTySecurityCipher;
```



## Basic Usage

### 1. Data Encryption

```java
String password = "MySuperSecurePassword";
String uuid = UUID.randomUUID().toString(); // Valid UUID
String plainText = "Top secret information I want to encrypt";

try {
    String encryptedText = ZTySecurityCipher.encrypt(password, uuid, plainText);
    System.out.println("Encrypted Text: " + encryptedText);
} catch (Exception e) {
    System.err.println("Encryption Error: " + e.getMessage());
}
```

#### What’s Happening Here?

1. **UUID Validation**: Throws an exception if the UUID is invalid.
2. **Random Salt and IV Generation**: Impossible to predict or reuse.
3. **Key Derivation**: PBKDF2 with HMAC-SHA512 and 5000 iterations.
4. **AES/GCM Encryption**: Protects both the confidentiality and integrity of the data.
5. **Final Output**: Returns Base64-encoded ciphertext for easy storage or transmission.



### 2. Data Decryption

```java
try {
    String decryptedText = ZTySecurityCipher.decrypt(password, uuid, encryptedText);
    System.out.println("Decrypted Text: " + decryptedText);
} catch (Exception e) {
    System.err.println("Decryption Error: " + e.getMessage());
}
```

#### Process Details:

1. **UUID Validation**: Must match the UUID used during encryption.
2. **Base64 Decoding**: Separates the combined data into salt, IV, and encrypted content.
3. **Key Derivation**: Reuses the same process as in encryption.
4. **AAD Authentication**: Verifies data integrity using the UUID.
5. **Decryption**: If authentication fails (due to tampering or incorrect UUID), an `AuthenticationException` is thrown.



## Custom Exceptions

- **SecurityInitializationException**: Critical error when initializing security components (rare).
- **SecurityOperationException**: General errors during encryption or decryption operations.
- **AuthenticationException**: Authentication failure, indicating possible data tampering or incorrect credentials.



## Security Recommendations

- Never reuse salts or IVs.
- Protect the password and UUID.
- Use Android Keystore if possible for more secure key management.
- Don’t rely solely on encryption—ensure additional security measures in your app.



## Complete Example

```java
public class SecurityExample {
    public static void main(String[] args) {
        String password = "StrongPassword123!";
        String uuid = "550e8400-e29b-41d4-a716-446655440000";
        String originalMessage = "This is ultra-secret information.";

        try {
            // Encryption
            String encryptedMessage = ZTySecurityCipher.encrypt(password, uuid, originalMessage);
            System.out.println("Encrypted: " + encryptedMessage);

            // Decryption
            String decryptedMessage = ZTySecurityCipher.decrypt(password, uuid, encryptedMessage);
            System.out.println("Decrypted: " + decryptedMessage);
        } catch (Exception e) {
            System.err.println("Security Error! " + e.getMessage());
        }
    }
}
```



## Conclusion

`ZTySecurityCipher` isn’t just another encryption class. It’s designed to frustrate anyone attempting to breach your data security. If decryption fails, someone is either trying to mess with your data—or you simply forgot the password.

Need security? You’ve got it. Just don’t lose the UUID or the password, because even I can’t save you then.
