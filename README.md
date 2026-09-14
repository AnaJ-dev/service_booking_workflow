Project: Service Booking Workflow

**
Purpose:
Customers select a service and an available appointment slot.

Connected systems:

- n8n Forms
- n8n Data Tables
- SMTP email
- Slack Incoming Webhook

Success path:

1. Customer submits details.
2. Workflow maps the service name to its internal ID.
3. Available slots are retrieved.
4. Customer chooses a slot.
5. Slot availability is checked again.
6. Booking record is created.
7. Slot status changes from available to booked.
8. Customer receives an email.
9. Team receives a Slack notification.

Failure cases:

- No slots are available when the customer starts booking.
- Selected slot becomes unavailable before final submission.

Required Data Tables:
Services_mapping:
serviceId, serviceName, isActive

TimeSlots:
slotId, serviceId, date, startTime, endTime, status

CustomerBooking:
firstName, lastName, email, phone, address, slotId, serviceId,
appointmentDate, startTime, endTime, bookingStatus, bookedAt


# Service Booking Workflow Map

```mermaid
flowchart TD

A[Customer submits booking form] --> B[Find service mapping]

B --> C[Get available time slots]

C --> D{Any slots available?}

D -->|No| E[Show no slots available]
D -->|Yes| F[Aggregate available slots]

F --> G[Customer chooses time slot]

G --> H[Extract selected slot ID]

H --> I[Recheck slot availability]

I --> J{Slot still available?}

J -->|No| K[Show slot no longer available]
J -->|Yes| L[Build booking record]

L --> M[Create booking in CustomerBooking table]

M --> N[Mark selected slot as booked]

N --> O[Send Slack notification]
N --> P[Send customer confirmation email]
```
