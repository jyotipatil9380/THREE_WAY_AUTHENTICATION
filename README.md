# THREE_WAY_AUTHENTICATION
"Three Way Authentication For Transaction" in this project we authenticate the user with three way first way is username and password where we use the client server architecture and here we use the dopost() method  ,second way is mobile otp generation where we use the API and get otp to registered mobile number and third way is scanner so stacktech=>JAVA JDBC API JSP SERVLET HTML CSS 
package com.threeway;

import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import javax.servlet.RequestDispatcher;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class RegistrationServlet extends HttpServlet {

    Connection con;

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        String uname = request.getParameter("name");
        String uemail = request.getParameter("email");
        String upassword = request.getParameter("password");

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            con = DriverManager.getConnection("jdbc:mysql://localhost:3306/login?useSSL=false", "root", "1234");

            PreparedStatement pst = con.prepareStatement("insert into users2(uname, upassword, uemail) values (?, ?, ?)");

            pst.setString(1, uname);
            pst.setString(2, upassword);
            pst.setString(3, uemail);

            int rowCount = pst.executeUpdate();

            if (rowCount > 0) {
                request.setAttribute("status", "success");
                request.setAttribute("msg", "User Registered Successfully");
                RequestDispatcher disp= request.getRequestDispatcher("success.jsp");
                disp.forward(request, response);
//                response.sendRedirect("success.jsp"); 
            } else {
                request.setAttribute("status", "failed");
                request.setAttribute("msg", "Failed to Registere");
               // RequestDispatcher disp= request.getRequestDispatcher("error.jsp");
                //disp.forward(request, response);
//                response.sendRedirect("error.jsp");
            }
        } catch (ClassNotFoundException | SQLException e) {
            // Print or log the exception stack trace
            e.printStackTrace();
        } finally {
            try {
                if (con != null) {
                    con.close();
                }
            } catch (SQLException ex) {
                // Print or log the exception stack trace
                ex.printStackTrace();
            }
        }
    }
}
