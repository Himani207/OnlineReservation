# OnlineReservation
import java.util.*;

class Reservation {
    static int pnrCounter = 1000;
    int pnr;
    String name;
    String trainNo;
    String trainName;
    String classType;
    String from;
    String to;
    String dateOfJourney;

    public Reservation(String name, String trainNo, String trainName, String classType,
                       String from, String to, String dateOfJourney) {
        this.pnr = pnrCounter++;
        this.name = name;
        this.trainNo = trainNo;
        this.trainName = trainName;
        this.classType = classType;
        this.from = from;
        this.to = to;
        this.dateOfJourney = dateOfJourney;
    }

    public void display() {
        System.out.println("PNR: " + pnr);
        System.out.println("Name: " + name);
        System.out.println("Train No: " + trainNo);
        System.out.println("Train Name: " + trainName);
        System.out.println("Class Type: " + classType);
        System.out.println("From: " + from);
        System.out.println("To: " + to);
        System.out.println("Date of Journey: " + dateOfJourney);
    }
}

public class OnlineReservationSystem {
    static Map<Integer, Reservation> reservations = new HashMap<>();
    static Scanner scanner = new Scanner(System.in);

    static Map<String, String> users = new HashMap<>() {{
        put("admin", "admin123");
        put("user", "pass123");
    }};

    public static void main(String[] args) {
        if (!login()) {
            System.out.println("Access Denied.");
            return;
        }

        while (true) {

        System.out.println("\n--- Online Reservation System ---");
            System.out.println("1. Reserve Ticket");
            System.out.println("2. Cancel Ticket");
            System.out.println("3. Exit");
            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();
            scanner.nextLine();  // Consume newline

            switch (choice) {
                case 1:
                    reserveTicket();
                    break;
                case 2:
                    cancelTicket();
                    break;
                case 3:
                    System.out.println("Exiting system. Thank you!");
                    return;
                default:
                    System.out.println("Invalid choice. Try again.");
            }
        }
    }

    public static boolean login() {
        System.out.println("--- Login Form ---");
        System.out.print("Enter Login ID: ");
        String loginId = scanner.nextLine();
        System.out.print("Enter Password: ");
        String password = scanner.nextLine();

        if (users.containsKey(loginId) && users.get(loginId).equals(password)) {
            System.out.println("Login Successful.");
            return true;
        } else {
            System.out.println("Invalid Login Credentials.");
            return false;
        }
    }

    public static void reserveTicket() {
        System.out.println("\n--- Reservation Form ---");
        System.out.print("Enter Your Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Train No: ");
        String trainNo = scanner.nextLine();
        String trainName = "Express " + trainNo; // dummy name generator
        System.out.print("Enter Class Type (e.g. Sleeper/AC): ");
        String classType = scanner.nextLine();
        System.out.print("Enter From: ");
        String from = scanner.nextLine();
        System.out.print("Enter To: ");
        String to = scanner.nextLine();
        System.out.print("Enter Date of Journey (YYYY-MM-DD): ");
        String date = scanner.nextLine();

        Reservation res = new Reservation(name, trainNo, trainName, classType, from, to, date);
        reservations.put(res.pnr, res);
        System.out.println("Reservation Successful! Your PNR is: " + res.pnr);
    }

    public static void cancelTicket() {
        System.out.println("\n--- Cancellation Form ---");
        System.out.print("Enter PNR Number: ");
        int pnr = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        if (reservations.containsKey(pnr)) {
            Reservation res = reservations.get(pnr);
            System.out.println("Reservation Details:");
            res.display();
            System.out.print("Are you sure you want to cancel? (yes/no): ");
            String confirm = scanner.nextLine();
            if (confirm.equalsIgnoreCase("yes")) {
                reservations.remove(pnr);
                System.out.println("Ticket Cancelled Successfully.");
            } else {
                System.out.println("Cancellation Aborted.");
            }
        } else {
            System.out.println("No reservation found with PNR: " + pnr);
        }
    }
}
