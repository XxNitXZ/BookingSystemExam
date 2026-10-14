# Booking System – Individual Coding Exam

A console application for managing meeting room bookings, developed as part of an individual practical coding exam.

The application is built with .NET 10 and includes automated xUnit tests.

## Requirements

- .NET 10 SDK

## Features

- Show active bookings
- Sort bookings by date and start time
- Search bookings by customer name
- Case-insensitive search
- Create new bookings
- Validate booking times
- Prevent overlapping bookings
- Validate room capacity
- Handle invalid participant input
- Cancel existing bookings
- Save and load bookings from JSON

## Run the application

Open the solution in Visual Studio and press the green **Play** button.

## Run the tests

Open **Test Explorer** in Visual Studio and select **Run All Tests**.

All provided automated tests should pass.

## Data storage

Bookings are stored locally in a JSON file.

The application reads and writes booking data through the file repository.

## Debugging report

A debugging report is available in:

DEBUGGING.md

The report describes how the booking overlap validation bug was investigated and fixed.
