# 2.4-23BCS12579

import java.sql.*;
public class ProductCRUD {
    private Connection con;

    public ProductCRUD() throws Exception {
        String url = "jdbc:mysql://localhost:3306/mydb";
        String username = "mydbuser";
        String password = "mydbuser";
        Class.forName("com.mysql.cj.jdbc.Driver");
        con = DriverManager.getConnection(url, username, password);
    }

    public void createProduct(int id, String name, double price) throws SQLException {
        String sql = "INSERT INTO product (id, name, price) VALUES (?, ?, ?)";
        PreparedStatement pst = con.prepareStatement(sql);
        pst.setInt(1, id); pst.setString(2, name); pst.setDouble(3, price);
        pst.executeUpdate(); pst.close();
    }

    public void readProducts() throws SQLException {
        Statement st = con.createStatement();
        ResultSet rs = st.executeQuery("SELECT * FROM product");
        while(rs.next()) {
            System.out.println(rs.getInt(1) + ", " + rs.getString(2) + ", " + rs.getDouble(3));
        }
        rs.close(); st.close();
    }

    public void updateProduct(int id, String name, double price) throws SQLException {
        String sql = "UPDATE product SET name=?, price=? WHERE id=?";
        PreparedStatement pst = con.prepareStatement(sql);
        pst.setString(1, name); pst.setDouble(2, price); pst.setInt(3, id);
        pst.executeUpdate(); pst.close();
    }

    public void deleteProduct(int id) throws SQLException {
        String sql = "DELETE FROM product WHERE id=?";
        PreparedStatement pst = con.prepareStatement(sql);
        pst.setInt(1, id); pst.executeUpdate(); pst.close();
    }

    public void close() throws SQLException { con.close(); }

    public static void main(String[] args) throws Exception {
        ProductCRUD productCrud = new ProductCRUD();
        productCrud.createProduct(1, "Pen", 10.5);
        productCrud.readProducts();
        productCrud.updateProduct(1, "Pen Updated", 11.0);
        productCrud.deleteProduct(1);
        productCrud.close();
    }
}
