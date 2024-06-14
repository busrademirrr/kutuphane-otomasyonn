import java.sql.*;

public class LibraryManagementSystem {
    private Connection conn;
    private Statement stmt;
    private ResultSet rs;

    public LibraryManagementSystem() {
        // Veritabanına bağlanma işlemi
        try {
            Class.forName("org.sqlite.JDBC");
            conn = DriverManager.getConnection("jdbc:sqlite:library.db");
            System.out.println("Veritabanına başarıyla bağlandı.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void login(String username, String password) {
        // Kullanıcı girişini doğrulama
        try {
            String query = "SELECT * FROM Users WHERE username=? AND password=?";
            PreparedStatement pstmt = conn.prepareStatement(query);
            pstmt.setString(1, username);
            pstmt.setString(2, password);
            rs = pstmt.executeQuery();

            if (rs.next()) {
                System.out.println("Giriş Başarılı!");
                // Giriş başarılı, burada yönlendirme veya işlevsellik ekleyebilirsiniz
            } else {
                System.out.println("Geçersiz Kullanıcı Adı veya Şifre!");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public void addBook(String title, String author, int publicationYear, String isbn, String status) {
        // Kitap ekleme işlemi
        try {
            String query = "INSERT INTO Books (title, author, publication_year, isbn, status) VALUES (?, ?, ?, ?, ?)";
            PreparedStatement pstmt = conn.prepareStatement(query);
            pstmt.setString(1, title);
            pstmt.setString(2, author);
            pstmt.setInt(3, publicationYear);
            pstmt.setString(4, isbn);
            pstmt.setString(5, status);
            pstmt.executeUpdate();
            System.out.println("Kitap başarıyla eklendi.");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public void updateBook(int bookId, String title, String author, int publicationYear, String isbn, String status) {
        // Kitap güncelleme işlemi
        try {
            String query = "UPDATE Books SET title=?, author=?, publication_year=?, isbn=?, status=? WHERE book_id=?";
            PreparedStatement pstmt = conn.prepareStatement(query);
            pstmt.setString(1, title);
            pstmt.setString(2, author);
            pstmt.setInt(3, publicationYear);
            pstmt.setString(4, isbn);
            pstmt.setString(5, status);
            pstmt.setInt(6, bookId);
            pstmt.executeUpdate();
            System.out.println("Kitap başarıyla güncellendi.");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public void deleteBook(int bookId) {
        // Kitap silme işlemi
        try {
            String query = "DELETE FROM Books WHERE book_id=?";
            PreparedStatement pstmt = conn.prepareStatement(query);
            pstmt.setInt(1, bookId);
            pstmt.executeUpdate();
            System.out.println("Kitap başarıyla silindi.");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public void listBooks() {
        // Tüm kitapları listeleme işlemi
        try {
            String query = "SELECT * FROM Books";
            Statement stmt = conn.createStatement();
            rs = stmt.executeQuery(query);

            while (rs.next()) {
                System.out.println("Kitap ID: " + rs.getInt("book_id"));
                System.out.println("Başlık: " + rs.getString("title"));
                System.out.println("Yazar: " + rs.getString("author"));
                System.out.println("Yayın Yılı: " + rs.getInt("publication_year"));
                System.out.println("ISBN: " + rs.getString("isbn"));
                System.out.println("Durum: " + rs.getString("status"));
                System.out.println("-----------------------------");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // Diğer işlevler buraya eklenebilir

    public static void main(String[] args) {
        LibraryManagementSystem librarySystem = new LibraryManagementSystem();
        // Test etmek için giriş yapabiliriz
        librarySystem.login("admin", "12345");
    }
}
