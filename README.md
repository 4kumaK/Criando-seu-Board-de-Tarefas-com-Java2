import java.sql.*;
import java.util.Scanner;

public class TaskBoard {
    private static final String DB_URL = "jdbc:mysql://localhost:3306/sampledb";
    private static final String USER = "root";
    private static final String PASS = "password"; // Altere para sua senha

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int option;

        do {
            System.out.println("Menu:");
            System.out.println("1. Criar novo bord");
            System.out.println("2. Selecionar bord");
            System.out.println("3. Excluir bord");
            System.out.println("4. Sair");
            System.out.print("Escolha uma opção: ");
            option = scanner.nextInt();
            scanner.nextLine(); // Consumir nova linha

            switch (option) {
                case 1:
                    createBoard(scanner);
                    break;
                case 2:
                    selectBoard(scanner);
                    break;
                case 3:
                    deleteBoard(scanner);
                    break;
                case 4:
                    System.out.println("Saindo...");
                    break;
                default:
                    System.out.println("Opção inválida. Tente novamente.");
            }
        } while (option != 4);
        
        scanner.close();
    }

    private static void createBoard(Scanner scanner) {
        System.out.print("Digite o nome do bord: ");
        String name = scanner.nextLine();
        String sql = "INSERT INTO boards (name) VALUES (?)";

        try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
             PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setString(1, name);
            pstmt.executeUpdate();
            System.out.println("Bord criado com sucesso!");
        } catch (SQLException e) {
            System.out.println("Erro ao criar bord: " + e.getMessage());
        }
    }

    private static void selectBoard(Scanner scanner) {
        System.out.print("Digite o ID do bord: ");
        int id = scanner.nextInt();
        String sql = "SELECT * FROM boards WHERE id = ?";

        try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
             PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setInt(1, id);
            ResultSet rs = pstmt.executeQuery();

            if (rs.next()) {
                System.out.println("Bord encontrado: " + rs.getString("name"));
            } else {
                System.out.println("Bord não encontrado.");
            }
        } catch (SQLException e) {
            System.out.println("Erro ao selecionar bord: " + e.getMessage());
        }
    }

    private static void deleteBoard(Scanner scanner) {
        System.out.print("Digite o ID do bord a ser excluído: ");
        int id = scanner.nextInt();
        String sql = "DELETE FROM boards WHERE id = ?";

        try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
             PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setInt(1, id);
            int rowsAffected = pstmt.executeUpdate();

            if (rowsAffected > 0) {
                System.out.println("Bord excluído com sucesso!");
            } else {
                System.out.println("Bord não encontrado.");
            }
        } catch (SQLException e) {
            System.out.println("Erro ao excluir bord: " + e.getMessage());
        }
    }
}
CREATE TABLE boards (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);


