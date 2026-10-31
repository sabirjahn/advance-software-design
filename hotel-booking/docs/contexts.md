# Contexts: the hotel's business areas

> DRAFT written from the P06 materials (the P05 steps 2–7 were not available when this was created).
> Compare it with your own P05 notes and change what differs.

## Words
Room, Guest, Stay, Reservation, Confirmed, Cancelled, Deposit, Paid, Declined, Message, Sent, Confirmation.

## Rooms
**Job:** knows which rooms the hotel sells and what type each one is.
**Words:** Room (number, type), room catalog.
**Rules it owns:** a room has a number and a type; the catalog lists every room the hotel sells.

## Reservations
**Job:** decides who has which room for which nights.
**Words:** Reservation, Stay (check-in, check-out), Guest, Confirmed, Cancelled.
**Rules it owns:** one room, one night, at most one guest (the booking rule); check-out must be after check-in; a cancelled booking no longer blocks the room.

## Payments
**Job:** takes deposits and says whether they were paid.
**Words:** Deposit, Paid, Declined.
**Rules it owns:** a positive amount can be Paid; anything else is Declined. (Booking does not call it yet: P17.)

## Notifications
**Job:** tells the guest what happened.
**Words:** Confirmation, Message, Sent.
**Rules it owns:** a failed message is reported, never hidden; how a message travels (email, SMS) is its own business.

## Same word, different meaning
| Word | In Rooms | In Reservations | In Payments / Notifications |
|---|---|---|---|
| Room | A thing the hotel sells (number, type) | The room a guest has for given nights | Notifications only needs the room number |
| Cancel | (not used) | The booking stops blocking the room | Payments: a deposit would be given back |
| Confirmed / Paid / Sent | – | Confirmed: the booking is valid | Paid: money arrived. Sent: a message left |

## Rule owners
| Rule | Owner | Why |
|---|---|---|
| One room, one night, at most one guest | Reservations | Only Reservations knows all bookings |
| Check-out must be after check-in | Reservations | It is about the stay |
| Which rooms exist | Rooms | It owns the catalog |
| A deposit is Paid or Declined | Payments | It talks to the payment provider |
| A failed message must not lose a booking | Reservations (keeps the booking) and Notifications (reports the failure) | The booking is saved first |

## Between contexts
| From → To | What crosses the border | Fields |
|---|---|---|
| Reservations → Rooms | Question: which rooms does the hotel sell? | (none) → list of Room (number, type) |
| Reservations → Notifications | Announcement: ReservationConfirmed | reservation id, guest email, room number, check-in, check-out |
| Reservations → Payments (from P17) | Question: take this deposit | reservation id, amount → Paid or Declined |
