# Requirements: hotel booking app, version 1

Not in version 1: prices, payments, invoices, logins, housekeeping.

A stay is counted in nights: the check-in day counts, the check-out day does not (half-open range).

## R1 · Search
A guest or receptionist can list the rooms that are free for given dates.
Check: empty hotel, 12–14 November → R7, R8.

## R2 · Book
A guest or receptionist can book a free room. The app answers with a reservation id.
Check: Maya books R7 for 12–14 November → Accepted R101.

## R3 · No clash
A room never has two Confirmed bookings on the same night. Check-out must be after check-in.
Check: Omar asks for R7 on 13–15 → Unavailable. 15–15 → InvalidDates.

More examples:
- Maya R7 12–14 (already booked) → Confirmed
- Omar R7 13–15 → Unavailable (night of the 13th)
- Nora R7 14–16 → Accepted (no shared night)
- R8 is already booked for 20–22 November. Request R8 22–24 → Accepted (check-out day 22 is free for a new guest)
- R8 is already booked for 20–22 November. Request R8 21–23 → Unavailable (night of the 21st)

## R4 · Cancel
A booking can be cancelled. It is kept as Cancelled and no longer blocks the room.
Check: Maya cancels R101 → R7 is free again for 12–14.

## R5 · View
A booking can be looked up by its id.
Check: R101 → Maya, R7, 12–14 November, Confirmed.

## R6 · A failed message keeps the booking
If the confirmation message cannot be sent, the booking is still Accepted and saved, and the answer says that the notification failed.
Check: the mail server is down, Maya books R7 for 12–14 November → Accepted R101, notification failed, 1 reservation saved.
