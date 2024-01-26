# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Airline Ticketing System
```
import random

MAX_FLIGHTS = 5
MAX_TICKETS_PER_FLIGHT = 80
MAX_DESTINATIONS = 6
PNR_LENGTH = 10

class Passenger:
    # Initialize Passenger object with empty attributes
    def __init__(self):
        self.passengerName = ""
        self.age = 0
        self.gender = ""


class Ticket(Passenger):
    # Initialize Ticket object, inheriting from Passenger, with additional attributes
    def __init__(self):
        self.seatNumber = 0
        self.fromDestination = ""
        self.toDestination = ""
        self.flightName = ""
        self.pnrNumber = ""
        self.ticketStatus = 0
        self.ticketClass = ""

def generate_pnr():
    '''
    Generates a random PNR (Passenger Name Record) number of specified length(in this case length of 10).
    '''
    charset = "0123456789"
    return ''.join(random.choice(charset) for _ in range(PNR_LENGTH))

def read_tickets_from_file(filename):
    '''
    Read ticket details from a file and return a list of dictionaries.
    Each dictionary represents the details of a ticket.
    '''
    tickets = []

    try:
        with open(filename, 'r') as file:
            current_ticket = {}
            inside_content = False

            for line in file:
                line = line.strip()

                if line.startswith("PNR Number:"):
                    inside_content = True
                    current_ticket = {}

                if inside_content:
                    if line.startswith("------"):
                        if current_ticket:
                            tickets.append(current_ticket)
                        inside_content = False
                    else:
                        key, value = map(str.strip, line.split(':', 1))
                        current_ticket[key] = value

    except FileNotFoundError:
        print(f"File '{filename}' not found.")

    

    return tickets


def load_ticket_price(from_dest, to_dest, ticket_class):
    '''
    Loads ticket price from a file based on the provided from_destination, to_destination, and ticket_class of the present ticket.
    '''
    with open("ticket_prices.txt", "r") as file:  
        for line in file:
            f_dest, t_dest, economy_price, business_price = line.split()
            if f_dest == from_dest and t_dest == to_dest:
                return float(economy_price) if ticket_class == "Economy Class" else float(business_price)
    

def save_passenger_details(ticket):
    '''
    Saves the passenger details to a file.
    '''
    with open("passengers.txt", "a") as file:
        file.write(f"PNR Number: {ticket.pnrNumber}\n")
        file.write(f"Passenger Name: {ticket.passengerName}\n")
        file.write(f"Age: {ticket.age}\n")
        file.write(f"Gender: {ticket.gender}\n")
        file.write(f"Seat Number: {ticket.seatNumber}\n")
        file.write(f"From Destination: {ticket.fromDestination}\n")
        file.write(f"To Destination: {ticket.toDestination}\n")
        file.write(f"Flight Name: {ticket.flightName}\n")
        file.write(f"Seat class: {ticket.ticketClass}\n")
        file.write("---------------------------------------------\n")

def tickets_of_passengers():
    '''
    Read ticket details from the passengers file and return a list of Ticket objects.
    '''
    tickets = []
    ticket_list = read_tickets_from_file('passengers.txt')

    if ticket_list:
        tickets = []
        for ticket in ticket_list:
            t = Ticket()
            t.pnrNumber = ticket['PNR Number']
            t.passengerName = ticket['Passenger Name']
            t.age = ticket['Age']
            t.gender = ticket['Gender']
            t.seatNumber = ticket['Seat Number']
            t.fromDestination = ticket['From Destination']
            t.toDestination = ticket['To Destination']
            t.flightName = ticket['Flight Name']
            t.ticketClass = ticket['Seat class']
            tickets.append(t)
    
    return tickets


def reserve_ticket(tickets, total_tickets , seats ,flight_names, destinations):
    '''
    Reserves a new ticket based on user input.
    '''
    if total_tickets >= MAX_FLIGHTS * MAX_TICKETS_PER_FLIGHT:
        print("Ticket reservation limit reached.")
        return

    ticket = Ticket()

    ticket.passengerName = input("Enter passenger name: ")
    ticket.age = int(input("Enter passenger age: "))
    ticket.gender = input("Enter passenger gender: ")

    print("Select a flight:")
    for i, flight in enumerate(flight_names, 1):
        print(f"{i}. {flight}")

    flight_choice = int(input("Enter your choice: "))
    if 1 <= flight_choice <= len(flight_names):
        ticket.flightName = flight_names[flight_choice - 1]
    else:
        print("Invalid flight choice.")
        return

    print("Select from destination:")
    for i, dest in enumerate(destinations, 1):
        print(f"{i}. {dest}")

    from_dest_choice = int(input("Enter your choice: "))
    if 1 <= from_dest_choice <= len(destinations):
        ticket.fromDestination = destinations[from_dest_choice - 1]
    else:
        print("Invalid from destination choice.")
        return

    print("Select to destination:")
    for i, dest in enumerate(destinations, 1):
        print(f"{i}. {dest}")

    to_dest_choice = int(input("Enter your choice: "))
    if 1 <= to_dest_choice <= len(destinations):
        ticket.toDestination = destinations[to_dest_choice - 1]
    else:
        print("Invalid to destination choice.")
        return

    if ticket.fromDestination == ticket.toDestination:
        print("Invalid destination selection. From and to destinations cannot be the same.")
        return

    print("Select ticket class:")
    print("1. Economy Class")
    print("2. Business Class")
    class_choice = int(input("Enter your choice: "))
    if class_choice == 1:
        ticket.ticketClass = "Economy Class"
        print("Economy Class selected.")
        ticket.pnrNumber = generate_pnr()
    elif class_choice == 2:
        ticket.ticketClass = "Business Class"
        print("Business Class selected.")
        ticket.pnrNumber = generate_pnr()
    else:
        print("Invalid class choice.")
        return

    price = load_ticket_price(ticket.fromDestination, ticket.toDestination, ticket.ticketClass)
    if price > 0:
        print(f"Ticket price from {ticket.fromDestination} to {ticket.toDestination}: {price}")
        payment = float(input("Enter the payable amount: "))
        if payment == price:
            print("Payment Successful.")
        else:
            print("Payment failed.")
            return
    else:
        return
    
    available_seat = -1
    count = 0
    for i in seats:
        if i[0] == ticket.flightName:
            count+=1
        if count >= MAX_TICKETS_PER_FLIGHT:
            available_seat = 0



    if available_seat == -1:
        for i in seats:
                ticket.seatNumber = random.randint(1, MAX_TICKETS_PER_FLIGHT)
                if i[0] == ticket.flightName and i[1] == ticket.seatNumber:
                    ticket.seatNumber = random.randint(1, MAX_TICKETS_PER_FLIGHT)
                else:
                    break
        
        ticket.ticketStatus = 1
        tickets.append(ticket)
        total_tickets += 1
        print("Ticket reserved successfully.")
        print(f"PNR Number: {tickets[total_tickets - 1].pnrNumber}")
        save_passenger_details(tickets[total_tickets - 1])

    else:
        print("No tickets available on this flight. Refund will be shortly credited to your account. Sorry for the inconvienience.")


def cancel_ticket():
    '''
    Cancels a ticket based on the provided PNR number.
    '''
    tickets = tickets_of_passengers()
    pnr = input("Enter PNR number: ")
    k = 1
    with open("passengers.txt", "w") as file:
        file.write("")
        
    for ticket in tickets:
        
        if ticket.pnrNumber != pnr:
            save_passenger_details(ticket)
        else:
            k=0
            with open("passengers.txt", "a") as file:
                file.write("")
            print(f"Ticket with PNR:{pnr} cancelled successfully.")
    if k==1:
        print("No ticket with this PNR number or this ticket is already canceled.")


def display_ticket():
    '''
    Displays details of a ticket based on the provided PNR number.
    '''
    tickets = tickets_of_passengers()
    pnr = input("Enter PNR number: ")
    k = 1
    for i in tickets:
        if pnr==i.pnrNumber:
            k = 0
            print("======================================")
            print("PNR Number:",i.pnrNumber)
            print("Passenger Name:",i.passengerName)
            print("Age:",i.age)
            print("Gender:",i.gender)
            print("Seat Number:",i.seatNumber)
            print("From Destination:",i.fromDestination)
            print("To Destination:",i.toDestination)
            print("Flight Name:",i.flightName)
            print("Seat class:",i.ticketClass)
            print("======================================")
    if k==1:
        print("No ticket reserved with this PNR or ticket already cancelled")
    return


def main():
    tickets = tickets_of_passengers()

 
    seats = [] # This Array contains the seats numbers of booked tickets in each flight
    for i in tickets:
        seats.append([i.flightName,i.seatNumber])


    flight_names = ["Indigo", "SpiceJet", "AirIndia", "DeccanAir", "Jet Airways"] # Available Flights
    destinations = ["Hyderabad(HYD)", "Chennai(CHE)", "Coimbatore(CBE)", "Bengaluru(BLR)", "Mumbai(BOM)", "Delhi(DEL)"] # Destinations

    choice = 0
    while choice != 4:
        print("\n========== Ticket Reservation System ==========")
        print("1. Reserve a ticket")
        print("2. Cancel a ticket")
        print("3. Display a ticket")
        print("4. Exit")
        choice = int(input("Enter your choice: "))

        if choice == 1:
            reserve_ticket(tickets, len(tickets), seats, flight_names, destinations)
        elif choice == 2:
            cancel_ticket()
        elif choice == 3:
            display_ticket()
        elif choice == 4:
            print("Exiting program.\nPlease visit again!!!")
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
```
