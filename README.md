# COBOL SEQ3000
**Table of Contents**

- [Summary](#summary)
- [System Flow](#system-flow)
- [Program Flow](#program-flow)
- [Running Output](#running-output)
- [Authors](#authors)

## Summary

The COBOL EMPLOYEE SYSTEM is a multi-program COBOL project designed to create, maintain, and update employee records using both sequential and indexed file processing techniques.

This system expands across multiple programs that work together to simulate real-world batch and online-style file maintenance. It includes initial file creation, sequential transaction processing, and indexed file maintenance with error handling. The programs demonstrate how employee data can be added, updated, deleted, and validated across different storage structures.

The system produces updated employee master files while also capturing invalid transactions for auditing and debugging purposes.


### System Flow

The system consists of three coordinated COBOL programs:

| Program | Role |
|---------|------|
| EMPIND01 | Creates an indexed employee master file from a sequential file |
| EMPIND02 | Maintains the indexed file using random access transactions |
| SEQ3000 | Processes transactions sequentially to produce a new master file |

Demonstrates both batch and interactive-style processing approaches.

### Transaction Types

Supports three transaction types using 88-level condition names:

| Type | Condition Name |
|------|----------------|
| Add | `ADD-RECORD` |
| Update | `CHANGE-RECORD` |
| Remove | `DELETE-RECORD` |

Applies business rules to determine valid vs. invalid operations. Invalid or failed transactions are written to **ERRTRAN** and **ERRTRAN3** for auditing.

### Program Flow

At startup, the system:
- Reads OLDEMP (sequential master file) and EMPTRAN (transaction file)
- Creates or opens the indexed file EMPMASTI for maintenance

For each transaction record, the program:
- Compares the transaction key against the master file key
- Detects match vs. no-match using merge/update logic
- On a match — applies ADD, CHANGE, or DELETE based on transaction type
- On no match — writes the existing master record as-is or flags the transaction as invalid
- Checks file status codes and writes error records to ERRTRAN when operations fail
- Accumulates control switches to track EOF and processing state

After all records are processed, the program:
- Writes the final master record(s)
- Closes all files and stops

### Running Output
No working output yet, as an "ABEND" error is recieved when running the JCL.

### 88-Level Switches Used

| Switch | Purpose |
|--------|---------|
| `TRANSACTION-EOF` | Marks end of transaction file |
| `MASTER-FOUND` | Indicates a matching master record was located |
| `ALL-RECORDS-PROCESSED` | Controls main processing loop termination |
| `NEED-TRANSACTION` | Signals the next transaction should be read |
| `WRITE-MASTER` | Triggers writing the current master record |

## Authors

- [@kayley-wells](https://github.com/kayley-wells) Kayley Wells
