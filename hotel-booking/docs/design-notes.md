# P03 design notes

## Why design before code
Low-level design decides which classes the app has, what information each class keeps, which methods it has, and which class calls which. We decide this before writing code so that every rule, like "is the room free?", is written in one place only.

## Use cases
Two actors use the app: the Guest (website) and the Receptionist (phone and walk-in guests). Both search, book and cancel; the receptionist also looks up bookings. Booking includes "check the room is free", which is where the core rule lives: one room, one night, at most one guest.

## Where the methods live
- Cancel() lives in Reservation, because cancelling changes the reservation's own status.
- HasOverlap(room, stay) lives in ReservationStore, because only the store can see all the reservations.

## Sequence and activity diagrams
A booking has two decisions: are the dates valid, and does a Confirmed reservation share a night? Only if both pass do we create, save, send the email and answer Accepted. The check and the save are two separate steps, and another request can arrive between them.

## The race: what went wrong
Maya and Omar both book R7 for 12-14 Nov at the same moment. Both requests checked before either one saved, so both were Accepted (R101 and R102) and 2 Confirmed reservations share the same nights. The core rule is broken. Version 1 does not fix it on purpose; P09 does.

# P04 design notes

## Refactoring
Refactoring changes how the code is organised, but not what it does.

Must stay the same:
- The tests from Step 2: all of them still pass after every move.
- The demo output: the same nine lines as in P03.
- The race demo: still "Double booking!" because refactoring does not fix bugs.

## What our code breaks
- D: BookingService asked for the concrete ReservationStore and EmailSender, so a database or SMS meant editing BookingService.
- S: all classes lived in one file.
- O: adding SMS meant changing BookingService instead of adding a new class.

## Target design
BookingService depends only on two interfaces: IReservationStore and INotificationSender. InMemoryReservationStore, EmailNotificationSender, SmsNotificationSender and FakeNotificationSender implement them (see the draw.io page "Classes - after").

# P06 design notes

## Before
- Tests: 8 passing.
- Demo: the nine lines from P03.
- Race: still double-books R7.

## Modules
| Module | Public contract (others may use) | Internal code (only the module, the wiring and the tests) |
|---|---|---|
| Rooms | Room, the room catalog promise | InMemoryRoomCatalog |
| Reservations | BookingService, BookingResult, Reservation, DateRange, Guest, ReservationStatus, the store promise | InMemoryReservationStore |
| Payments | PaymentStatus, the payment service promise | SimulatedPaymentService |
| Notifications | Confirmation, the notification sender promise | EmailNotificationSender, SmsNotificationSender |

## Architecture rules
1. Reservations uses only the public contracts of other modules: BookingService may use the Notifications contract, never the email sender.
2. Rooms does not depend on any other module.
3. Payments and Notifications do not depend on Reservations: the arrows point one way, so there is no dependency circle.

## Broken on purpose
(Fill this in after you run Step 7 yourself.) Rule 1 failed: lint-imports said hotel_booking.reservations.booking is not allowed to import hotel_booking.notifications.internal.

## After
- Tests: 8 passing, plus 3 architecture contracts kept.
- Demo: the same nine lines as before.
- Race: still double-books R7.
