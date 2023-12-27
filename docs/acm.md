# Autonomous Control Message (ACM)

Custom message structure used for:
- Sending data between processes/threads
- Logging data
- Replaying data

## Structure
Every ACM contains:
- 64bit header
  - split into 2x 32bit integers
        - 32 bits for the data length (DL)
        - 32 bits for the message identifier (ID)
- 64bit timestamp of the message (nanoseconds) 
- N bit payload
    - The actual data
    - Technically the max length is 2^32

An ACM is framed using:
- 2 bytes (16bit) at the start, typically referred to as start bytes
    - 0x65
    - 0x41
- 2 bytes (16bit) at the end, the CRC
    - CRC is calculated from data length to the last payload byte
    - Does not include start bytes or the CRC bytes

## Notes
- All ACM's need to be word aligned
    - This is done so we can simply memcpy or cast messages into a more use    