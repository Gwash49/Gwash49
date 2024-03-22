
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.sql.*;

public class RegistrationForm extends JFrame implements ActionListener {
    JLabel nameLabel, mobileLabel, genderLabel, dobLabel, addressLabel, termsLabel;
    JTextField nameField, mobileField, dobField, addressField;
    JRadioButton maleRadioButton, femaleRadioButton;
    JButton submitButton;
    JTextArea displayArea;

    public RegistrationForm() {
        setTitle("Registration Form");
        setLayout(new GridLayout(8, 2));

        nameLabel = new JLabel("Name");
        nameField = new JTextField(20);

        mobileLabel = new JLabel("Mobile");
        mobileField = new JTextField(20);

        genderLabel = new JLabel("Gender");
        maleRadioButton = new JRadioButton("Male");
        femaleRadioButton = new JRadioButton("Female");
        ButtonGroup genderGroup = new ButtonGroup();
        genderGroup.add(maleRadioButton);
        genderGroup.add(femaleRadioButton);

        dobLabel = new JLabel("DOB");
        dobField = new JTextField(20);

        addressLabel = new JLabel("Address");
        addressField = new JTextField(20);

        termsLabel = new JLabel("Accept Terms and Conditions");
        JCheckBox termsCheckbox = new JCheckBox();

        submitButton = new JButton("Submit");
        submitButton.addActionListener(this);

        displayArea = new JTextArea();
        displayArea.setEditable(false);

        add(nameLabel);
        add(nameField);
        add(mobileLabel);
        add(mobileField);
        add(genderLabel);
        add(maleRadioButton);
        add(new JLabel(""));
        add(femaleRadioButton);
        add(dobLabel);
        add(dobField);
        add(addressLabel);
        add(addressField);
        add(termsLabel);
        add(termsCheckbox);
        add(submitButton);
        add(displayArea);

        pack();
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }

    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == submitButton) {
            // Code to store data in database
            String name = nameField.getText();
            String mobile = mobileField.getText();
            String gender = maleRadioButton.isSelected() ? "Male" : "Female";
            String dob = dobField.getText();
            String address = addressField.getText();

            try {
                Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydatabase", "username", "password");
                Statement stmt = con.createStatement();
                String query = "INSERT INTO users (name, mobile, gender, dob, address) VALUES ('" + name + "', '" + mobile + "', '" + gender + "', '" + dob + "', '" + address + "')";
                stmt.executeUpdate(query);
                con.close();

                displayArea.setText("Data saved successfully!");
            } catch (Exception ex) {
                System.out.println(ex);
                displayArea.setText("Error saving data");
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                new RegistrationForm().setVisible(true);
            }
        });
    }
}