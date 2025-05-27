# sBTC-ChainIPProtect - Intellectual Property Protection Smart Contract

## Overview

This Smart Contract provides a robust and secure system for registering, verifying, and managing Intellectual Property (IP) assets on the blockchain. It supports:

* Registration of IP assets with cryptographic hashes.
* Optional expiration dates for IP registrations.
* Verification of IP ownership and hash authenticity.
* Secure update of IP metadata (hash).
* Extension of IP registration expiration dates.
* Ownership verification with strict authorization checks.
* Prevention of duplicate IP hash registrations.
* Optimized error handling and input validation.

This contract is designed to provide a transparent, tamper-proof, and efficient way to protect and manage IP rights on-chain.

---

## Features

### 1. Register IP Asset

* Register a new IP asset by providing a 32-byte cryptographic hash representing the IP.
* Optionally specify an expiration block height after which the registration expires.
* Prevents duplicate IP hashes from being registered.
* Assigns a unique incremental ID to each IP registration.

### 2. Verify IP Owner

* Query the current owner of an IP asset by its ID.
* Returns error if the IP registration is expired or does not exist.

### 3. Verify IP Hash

* Verify if the given hash matches the registered hash for a specific IP ID.
* Checks if the IP registration is active (not expired).

### 4. Update IP Metadata

* Allows the owner of an IP asset to update the associated hash.
* Requires the registration to be active and ownership verified.
* Updates the hash registry to prevent duplicates.

### 5. Extend IP Registration

* Allows the owner to extend the expiration block height of their IP registration.
* New expiration must be greater than the current block and any previously set expiration.

### 6. Check Hash Registration

* Read-only function to check if a hash is already registered in the system.

---

## Error Codes

| Error Code                    | Description                                      |
| ----------------------------- | ------------------------------------------------ |
| `ERR-NOT-AUTHORIZED`          | Caller is not authorized to perform the action.  |
| `ERR-INVALID-HASH-LENGTH`     | IP hash length is invalid (must be 32 bytes).    |
| `ERR-HASH-ALL-ZEROS`          | IP hash cannot be all zeros.                     |
| `ERR-HASH-ALREADY-REGISTERED` | The IP hash is already registered.               |
| `ERR-IP-NOT-FOUND`            | IP ID not found in registration map.             |
| `ERR-INVALID-IP-ID`           | Provided IP ID is invalid (0 or out of range).   |
| `ERR-IP-ID-OUT-OF-RANGE`      | Provided IP ID exceeds the current counter.      |
| `ERR-IP-EXPIRED`              | IP registration has expired.                     |
| `ERR-INVALID-EXPIRATION`      | Expiration block is invalid (must be in future). |
| `ERR-NO-EXPIRATION-SET`       | No expiration date set for this IP registration. |

---

## Contract Storage

* `owner` — The contract owner (deployer).
* `ip-registrations` — Map from IP ID to IP metadata (`owner`, `timestamp`, `hash`, and optional `expiration`).
* `registered-hashes` — Map from IP hash to IP ID, used to ensure uniqueness.
* `ip-counter` — A counter to assign new unique IP IDs incrementally.

---

## Usage

### Register a new IP

```clarity
(register-ip ip-hash (optional expiration-block))
```

* `ip-hash`: A 32-byte buffer representing the IP asset.
* `expiration-block`: (optional) Block height at which registration expires.

### Verify IP owner by ID

```clarity
(verify-ip-owner ip-id)
```

Returns owner principal and expiration if active.

### Verify IP hash by ID

```clarity
(verify-ip-hash ip-id hash-to-verify)
```

Returns `true` if the hash matches the registered hash for the IP ID and is not expired.

### Update IP metadata

```clarity
(update-ip-metadata ip-id new-hash)
```

Only the owner can update the hash if the IP registration is active.

### Extend IP registration expiration

```clarity
(extend-ip-registration ip-id new-expiration)
```

Only the owner can extend the expiration date.

### Check if a hash is registered

```clarity
(is-hash-registered ip-hash)
```

Returns `true` if the hash exists in the system.

---

## Deployment

* Deploy the contract on a Clarity-compatible blockchain (e.g., Stacks).
* The deployer becomes the `owner`.
* Users can then interact with the contract to register and manage their IP assets.

---

## Security Considerations

* Strict owner verification before allowing updates or expiration extensions.
* Prevents duplicate IP hash registrations to maintain uniqueness.
* Validates expiration blocks to ensure they are in the future.
* Proper error handling provides clear feedback for invalid actions.
* Use a trusted cryptographic hash function (e.g., SHA-256) off-chain to generate IP hashes before registration.

---

## License

Specify your license here (e.g., MIT License).


---
