# PhonebookEntry
This program uses composition of the Name and PhoneNumber classes to put a name with a number in a phonebook and then dial the numbers (only if its not a duplicate entry).

import java.util.Scanner;
import java.io.*;

public class PhonebookEntry {
    private Name name;
    private PhoneNumber phoneNumber;
    public PhonebookEntry (Name name, PhoneNumber num) {
        this.name = name;
        this.phoneNumber = num;
    }
    public Name getName () {
        return name;
    }
    public PhoneNumber getPhoneNumber () {
        return phoneNumber;
    }
    public void call () {
        if(phoneNumber.isTollFree()) {
            System.out.println("Dialing (toll-free) " + name.toString() + ": "  + phoneNumber.toString());
        }
        else {
            System.out.println("Dialing " + name.toString() +  ": "  + phoneNumber.toString());
        }
    }
    public String toString() {return name.toString() + phoneNumber.toString();}
    public boolean equals(PhonebookEntry other)  {
        return name.equals(other.name) && phoneNumber.equals(other.phoneNumber);}
    public static PhonebookEntry read (Scanner scanner) {
        if (!scanner.hasNext()) return null;
        Name name = Name.read(scanner);
        PhoneNumber phoneNumber = PhoneNumber.read(scanner);
        return new PhonebookEntry (name, phoneNumber);
    }
    public static void main (String [] arg ) throws IOException {
        Scanner inFile = new Scanner(new File("phonebook.text"));
        int count = 0;
        PhonebookEntry entry = read(inFile);
        PhonebookEntry other = null;
        while(entry != null) {
            if(other != null && entry.name.equals(other.name) && !(entry.phoneNumber.equals(other.phoneNumber))) {
                System.out.println("Warning duplicate name encountered \"" + entry.name + ": " + entry.phoneNumber +
                        "\" discovered");
            }
            if(other == null || !(entry.equals(other))) {
                System.out.println("phone book entry: " + entry.getName() + ": " + entry.getPhoneNumber());
                entry.call();
                System.out.println();
            }
            else if(entry.equals(other)) {
                System.out.println("Duplicate phone book entry \"" + entry.name + ": " + entry.phoneNumber +
                        "\" discovered");
            }
            count++;
            other = entry;
            entry = read(inFile);
        }
        System.out.println("---");
        System.out.println(count + " names processed");
    }
}
