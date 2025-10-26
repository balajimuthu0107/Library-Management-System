import java.util.*;

class Book{
    String title;
    String Author;
    String genre;
    boolean isAvailable;

    Book(String title,String Author,String genre){
        this.title=title;
        this.Author=Author;
        this.genre=genre;
        this.isAvailable=true;//start stage all books are available
    }

}

class Member{
    String username;
    List<Book> borrowedBooks;

    Member(String username){
        this.username = username;
        this.borrowedBooks = new ArrayList<>();
        }
    boolean BorrowBook(Book book){
        if (borrowedBooks.size() < 5 && book.isAvailable){
            borrowedBooks.add(book);
            book.isAvailable=false;
            return true;
        }
        return false;
    }
    void returnBook(Book book){
        borrowedBooks.remove(book);
        book.isAvailable=true;
    }
}


public class Main{
    static Scanner sc = new Scanner(System.in);
    static List<Book> books= new ArrayList<>();
    static  List<Member>members = new ArrayList<>();
    static Member loggedInMember = null;
    static  final String adminuser ="admin";
    static final String adminpassword="password";

    public static void main(String[] args) {
        books.add(new Book("Maths",  "suba",  "educational"));
        books.add(new Book("Rich dad poor dad", "null", "store"));

        while(true){
            System.out.print("Enter your role(admin/user):");
            String role = sc.nextLine();

            if (role.equals("admin")){
                loginAsAdmin();
            }
            else if (role.equals("user")) {
                loginAsUser();
            }
            else {
                System.out.println("Invalid role.please try again");
            }
        }
    }
    private static void loginAsAdmin(){
        System.out.print("Enter Username:");
        String username = sc.nextLine();
        System.out.print("enter Password:");
        String password = sc.nextLine();

        if (username.equals(adminuser)&&password.equals(adminpassword)){
            System.out.println("Login as admin.");
            adminMenu();
        }
        else {
            System.out.println("invalid admin.");
        }
    }
    private static void loginAsUser(){
        System.out.print("Enter Username:");
        String username = sc.nextLine();
        loggedInMember = findOrCreateMember(username);
        System.out.println("Login as user.");
        userMenu();
    }
    private static Member findOrCreateMember(String username){
        for (Member member:members) {
            if (member.username.equals(username)) {
                return member;
            }
        }
        Member newMember = new Member(username);
        members.add(newMember);
        return newMember;

    }
    private static void adminMenu(){
        while (true){
            System.out.println("\nLibrary Management System");
            System.out.println("1.Add Book");
            System.out.println("2.Updated Book");
            System.out.println("3.Remove Book");
            System.out.println("4.Add Member");
            System.out.println("5.Display All Books");
            System.out.println("6.Display All Member");
            System.out.println("7.Exit");

            System.out.print("Enter a Choice:");
            int choice = sc.nextInt();
            sc.nextLine();

            switch (choice){
                case 1:
                    addBook();
                    break;
                case 2:
                    updatedBook();
                    break;
                case 3:
                    removeBook();
                    break;
                case 4:
                    addMember();break;
                case 5:
                    displayAllBook();
                    break;
                case 6:
                    displayAllMember();
                    break;
                case 7:
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }

        }
    }
    private static void userMenu(){
        while (true){
            System.out.println("\nLibrary Management System");
            System.out.println("1. Borrow Book");
            System.out.println("2. Return Book");
            System.out.println("3.Display All Books");
            System.out.println("4.Display All Member");
            System.out.println("5.Exit");

            System.out.println("Enter a choice");
            int choice = sc.nextInt();
            sc.nextLine();

            switch (choice){
                case 1:
                    borrowBook();break;
                case 2:
                    returnBook();break;
                case 3:
                    displayAllBook();break;
                case 4:
                    displayAllMember();break;
                case 5:
                    return;
                default:
                    System.out.println("Invalid choice .pleace try again");
            }

        }
    }
    private static void addBook() {
        System.out.print("Enter Book Title:");
        String title = sc.nextLine();
        System.out.print("Enter Author Name:");
        String author = sc.nextLine();
        System.out.print("Enter a genre:");
        String genre = sc.nextLine();

        books.add(new Book(title,author,genre));
        System.out.println("Book added succesfully.");
    }
    private static void updatedBook(){
        System.out.println("Enter Book title To Update:");
        String title = sc.nextLine();

        Book book = findBookByTitle(title);
        if (book != null){
            System.out.println("Enter new Author:");
            book.Author = sc.nextLine();
            System.out.println("Enter a Genre:");
            book.genre=sc.nextLine();
            System.out.println("Book Updated Successfully.");
        }
        else {
            System.out.println("Book Not Found.");
        }
    }
    private static void removeBook(){
        System.out.println("Enter a book title to remove:");
        String title = sc.nextLine();

        Book book = findBookByTitle(title);
        if (book != null){
            books.remove(book);
            System.out.println("Book Removed Successfully.");
        }
        else{
            System.out.println("Book Not Found.");
        }
    }
    private static Book findBookByTitle(String title){
        for (Book book :books){
            if (book.title.equalsIgnoreCase(title)){
                return book;
            }
        }
        return null;
    }
    private static void addMember(){
        System.out.println("Enter Username Member:");
        String Username = sc.nextLine();
        members.add(new Member(Username));
        System.out.println("Member added Successfully.");
    }
    private static void borrowBook(){
        System.out.println("Enter Book Title To Borrow:");
        String title = sc.nextLine();

        Book book = findBookByTitle(title);
        if (book != null && loggedInMember.BorrowBook(book)){
            System.out.println("Book Borrowed Successfully");

        }
        else{
            System.out.println("Book Is Not Avaliable or borrow limit Reached.");
        }
    }
    private static void returnBook(){
        System.out.println("Enter Book title to Return:");
        String title = sc.nextLine();

        Book book = findBookByTitle(title);
        if(book != null&& !book.isAvailable){
            loggedInMember.returnBook(book);
            System.out.println("Book Returned Successfully.");
        }
        else {
            System.out.println("Book not found or it Wasn't borrowed.");
        }
    }
    private static void displayAllBook(){
        if (books.isEmpty()){
            System.out.println("Book Not Found.");
        }
        else{
            System.out.println("\n List Of Book");
            for (Book book:books){
                System.out.println("title:"+book.title+" Author:"+book.Author+ " gemre"+book.genre+"Available"+book.isAvailable);

            }
        }
    }
    private static void displayAllMember(){
        if (members.isEmpty()){
            System.out.println("Memners Not Exist.");
        }
        else {
            System.out.println("\nList Of Members:");
            for (Member member:members){
                System.out.println("Username:"+member.username+"Borrowed Book:"+member.borrowedBooks.size());

            }
        }
    }

}
