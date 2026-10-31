# Quality scenarios

## Stakeholders

| Who | What they care about |
|---|---|
| Guest | Finds a free room quickly; never loses a booking |
| Receptionist | Never promises one room twice; can cancel at the desk |
| Hotel manager (domain expert) | Knows the rules; wants deposits and SMS soon |
| Development team | Can change one area without breaking the others |
| Email/SMS and payment providers | Outside systems that can be slow or down |

## S1 · Performance: 200 guests search at once

| Part | S1 |
|---|---|
| Source | 200 guests |
| Stimulus | They search free rooms for 12–14 Nov |
| Environment | Within one minute |
| Artifact | The free-room search in BookingService |
| Response | Correct free rooms are shown |
| Measure | p95 response ≤ 1 second |
| Checked in | P08 (cache), P23 (load test) |

## S2 · Data integrity: no double booking

| Part | S2 |
|---|---|
| Source | 100 pairs of guests |
| Stimulus | Each pair books the same room and nights at the same moment |
| Environment | Normal operation |
| Artifact | BookingService.Reserve and the reservation store |
| Response | Exactly one Confirmed reservation per pair |
| Measure | 0 double bookings |
| Checked in | P09 |

## S3 · Availability: a failed message keeps the booking

| Part | S3 |
|---|---|
| Source | The email provider |
| Stimulus | It stops answering |
| Environment | For 10 minutes, during normal bookings |
| Artifact | BookingService.Reserve and the notification sender |
| Response | Every valid booking is still Accepted and saved; the failed message is reported |
| Measure | 0 lost bookings; every failed message is counted |
| Checked in | P04 (R6 test, done) and P10 |

## S4 · Security: nobody reads another guest's reservation

| Part | S4 |
|---|---|
| Source | A guest |
| Stimulus | Tries 100 times to read another guest's reservation |
| Environment | Normal operation |
| Artifact | The look-up of a booking by id |
| Response | No reservation of another guest is shown |
| Measure | 0 reservations shown to the wrong guest |
| Checked in | P21 |

## S5 · Modifiability: adding SMS

| Part | S5 |
|---|---|
| Source | The hotel |
| Stimulus | It adds SMS confirmations |
| Environment | During development |
| Artifact | BookingService and the notification sender |
| Response | BookingService is not edited |
| Measure | At most 2 files change |
| Checked in | P04 (SMS sender, done) |

## S6 · Sustainability: less energy per search

| Part | S6 |
|---|---|
| Source | The hotel's operator |
| Stimulus | The same 1,000 searches run before and after a change |
| Environment | Same machine, same data |
| Artifact | The free-room search |
| Response | The same searches use less energy |
| Measure | At least 20% fewer watt-hours per 1,000 completed searches |
| Checked in | P08, P24 |
