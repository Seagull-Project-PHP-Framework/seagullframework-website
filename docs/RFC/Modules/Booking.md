<!-- Name: RFC/Modules/Booking -->
<!-- Version: 7 -->
<!-- Last-Modified: 2006/03/14 15:18:42 -->
<!-- Author: werner -->
# Generic Booking Module
This RFC is about a generic booking module that can be extended to fit your specific needs (e.g. booking of concerts, hotels, services...)

[[TOC]]
## What do we need
we need a front-office (where customers can book) and a backoffice (for managing things to book)
### front office
  * form for booking
  * listing if subject is free
### back office
  * managing subjects
  * managing customers
  * managing bookings / contracts

## possible bookflow
  * A customer fills out a form to do the booking (maybe a calendar shows availibility)
    * either online payment and subject is booked
    * either additional offer by seller, acceptance by buyer, ok by seller, subject is booked

## bookable object
There are different kinds of bookable objects:
  * e.g. concerts/travels: one date, can be booked by N persons (e.g. 100 tickts for the concert on 1/1/2006).
    * one concert (band foo in city hall) can be on more than one date
  * e.g. hotel room: Can be booked by 1-N Persons, isn't available when one Person has booked the room. Is bookable on every day the hotel is open, bookable per night.
  * e.g. service: can be booked per hour (or any other minimal unit) every day/workday from 9am until 5pm

## db structure
We need tables for
  * user 1:n objects relationship: (which object can be administrated by which user)
  * objects: (data for objects, like hotel, flat, restaurant.)
  * subobjects: (e.g. room of a hotel)
  * contracts: (which customer has booked which object)
  * booking: which object is booked from-to
  * price / availiblility: room (hotel foo, single room (or room #23) is available from-to for price $$)

## needed methods
We need methods for:
  * customer can book something

  * admin can manage bookings

  * show new bookings
  * show  all bookings (history)
  * list customers
  * add / edit / delete booking -> add / edit / delete contract
  * manage objects and subobjects
  * add / edit / delete objects
  * add avalibility / price
  * manage last minute discounts
  * statistically functions (useful to have)

### BookingMgr
Frontend for customers:

  * listAll 
    * lists all items, either from all vendors or all items of one vendor
    * sortable: categories, location etc...
  * showForm
  * send

### VendorMgr
-> can this be made by OrgMgr in user module?

  * list
  * add / edit / delete

### PropertyMgr
for managing the properties / objects of vendors

  * list
  * add / edit / delete

### PriceMgr
guess what??

  * list
  * add / edit / delete
  * setDiscount

### BookingAdminMgr

  * setStatus (available,pending, booked)
  * add/edit/delete
  * list


### ContractMgr
if contracts are needed to book something. in hotel booking this would be needed.