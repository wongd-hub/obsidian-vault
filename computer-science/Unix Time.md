#course_cs50

- A way that we represent time is with a signed integer of 32 bits tracking the number of seconds since 00:00:00 UTC on 1 January 1970 (the _Unix epoch_).
    - This means that we can track roughly 68 years before and after the Unix epoch
    - We can only track up to Tuesday 2038-01-19; one second after the last representable second, the integer will overflow back to 13 December 1901, presenting another Y2K scenario.