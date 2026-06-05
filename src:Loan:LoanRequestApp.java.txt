package Loan;
import javax.swing.*;
import java.io.*;

import javax.swing.table.DefaultTableModel;

import java.awt.*;

public class LoanRequestApp {

	public static void main(String[] args) {
		
		// Create the main application window
		JFrame frame = new JFrame("Bank Loan Request Management System");
		frame.setSize(900,600);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.setLocationRelativeTo(null);
		
		// Create and customize the application title
		JLabel title = new JLabel("Bank Loan Request Management System");
		title.setHorizontalAlignment(JLabel.CENTER);
		title.setFont(new Font("Arial", Font.BOLD , 24));
		frame.add(title,BorderLayout.NORTH);
		
		// Create the form panel using GridLayout
		JPanel panel = new JPanel();
		panel.setLayout(new GridLayout(17 , 2 , 10 , 10));
		
		// Create labels for client and loan information
		JLabel Client = new JLabel("Client Name");
		JLabel CIN = new JLabel("CIN");
		JLabel month = new JLabel("Monthly Income");
		JLabel loan = new JLabel("Loan Amount");
		JLabel duration = new JLabel("Duration");
		JLabel interset = new JLabel("Interest Rate (%)");
		JLabel result = new JLabel("Result : ");
		JLabel loanType = new JLabel("Loan Type");
		JLabel searchLabel = new JLabel("Search by CIN");
		
		// Create text fields to enter data
		JTextField client_field = new JTextField();
		JTextField cin_field = new JTextField();
		JTextField month_field = new JTextField();
		JTextField loan_field = new JTextField();
		JTextField duration_field = new JTextField();
		JTextField interest_field = new JTextField();
		JTextField search_field = new JTextField();
		
		// Create buttons for application actions
		JButton calc = new JButton("Calculate Monthly Payment");
		JButton request = new JButton("Add Request");
		JButton approve = new JButton("Approve");
		JButton reject = new JButton("Reject");
		JButton clear = new JButton("Clear Fields");
		JButton delete = new JButton("Delete");
		JButton search = new JButton("Search");
		JButton save = new JButton("Save Data");
		
		 // Create a combo box for selecting the type of loan
		String[] loanTypes = {"Personal Loan", "Car Loan", "House Loan", "Business Loan"};
		JComboBox<String> loanTypeBox = new JComboBox<>(loanTypes);

		// Add all form components to the panel
		panel.add(Client);
		panel.add(client_field);

		panel.add(CIN);
		panel.add(cin_field);

		panel.add(month);
		panel.add(month_field);

		panel.add(loan);
		panel.add(loan_field);

		panel.add(loanType);
		panel.add(loanTypeBox);

		panel.add(duration);
		panel.add(duration_field);

		panel.add(interset);
		panel.add(interest_field);
		
		panel.add(searchLabel);
		panel.add(search_field);

		panel.add(search);
		panel.add(new JLabel(""));
		
		panel.add(save);
		panel.add(new JLabel(""));
	
		// Add the form panel to the left side of the window
		frame.add(panel, BorderLayout.WEST);
		
		// Calculate the estimated monthly payment
		calc.addActionListener(e ->{
			try {
				double loanAMount = Double.parseDouble(loan_field.getText());
				int months = Integer.parseInt(duration_field.getText());
				double interestRate = Double.parseDouble(interest_field.getText());
				
				double totalInterest = loanAMount * interestRate / 100;
				double totalAmount = loanAMount + totalInterest;
				double monthlyPayment = totalAmount / months ;
				
				result.setText("Monthly Payment: " + String.format("%.2f", monthlyPayment));
				
			}catch(Exception ex) {
				JOptionPane.showMessageDialog(frame,"Please enter valid numbers");
				
			}
		});
		panel.add(calc);
		panel.add(new JLabel(""));
		
		panel.add(result);
		panel.add(new JLabel(""));
		
		// Create table columns
		String [] columns = {
				"Client Name",
				"CIN",
				"Income",
				"Loan Amount",
				"Loan Type",
				"Duration",
				"Interest ",
				"Monthly Payment",
				"Status"
		};
		
        // Create the table that will display loan requests
		DefaultTableModel tableModel = new DefaultTableModel(columns , 0);
		JTable table = new JTable(tableModel);
		JScrollPane scrollPane = new JScrollPane(table);
		frame.add(scrollPane , BorderLayout.CENTER);
		
		panel.add(request);
		panel.add(new JLabel(""));
		
		
		// Add a new loan request to the table
		request.addActionListener(e ->{
			
			String clientName = client_field.getText();
			String cin = cin_field.getText();
			String income = month_field.getText();
			String loanAmount = loan_field.getText();
			String Duration = duration_field.getText();
			String Interest = interest_field.getText();
			String monthlyPayment = result.getText();
			String loantype = loanTypeBox.getSelectedItem().toString();
			
			 // Check if all fields are filled
			if (clientName.isEmpty() || cin.isEmpty() || income.isEmpty() || loanAmount.isEmpty()
			        || Duration.isEmpty() || Interest.isEmpty()) {
			    
			    JOptionPane.showMessageDialog(frame, "Please fill all the fields.");
			    return;
			}
			
			// Validate that CIN contains exactly 8 numbers
			if (!cin.matches("\\d{8}")) {
			    JOptionPane.showMessageDialog(frame, "CIN must contain exactly 8 numbers.");
			    return;
			}
			
			// Add the request data to the table with Pending status
			tableModel.addRow(new Object[] {
				    clientName,
				    cin,
				    income,
				    loanAmount,
				    loantype,
				    Duration,
				    Interest + "%",
				    monthlyPayment,
				    "Pending"
				});
		
		
		
		});
		
		// Change selected request status to Approved
		approve.addActionListener(e ->{
			int selectedRow = table.getSelectedRow();
			if(selectedRow == -1){
				JOptionPane.showMessageDialog(frame, "Please Select a request first.");
				return;
			}
			tableModel.setValueAt("Approved", selectedRow, 8);
		});
		
		reject.addActionListener(e ->{
			int selectedRow = table.getSelectedRow();
			if (selectedRow == -1) {
				JOptionPane.showMessageDialog(frame, "Please select a request first.");
				return;
			}
			tableModel.setValueAt("Rejected", selectedRow, 8);
		});
		panel.add(approve);
		panel.add(new JLabel(""));
		
		panel.add(reject);
		panel.add(new JLabel(""));
		
		// Clear all input fields
		clear.addActionListener(e ->{
			client_field.setText("");
			cin_field.setText("");
			month_field.setText("");
			loan_field.setText("");
			duration_field.setText("");
			interest_field.setText("");
			result.setText("Monthly Payment :");
		});
		panel.add(clear);
		panel.add(new JLabel(""));
		
		// Delete the selected loan request after confirmation
		delete.addActionListener(e ->{
			int selectedRow = table.getSelectedRow();
			if(selectedRow == -1) {
				JOptionPane.showMessageDialog(frame,"Please select a request to delete");
				return;
			}
			int confirm = JOptionPane.showConfirmDialog(frame, "Are you sure you want to delete this request?",
					"Confirm Delete",JOptionPane.YES_NO_OPTION);
			if(confirm == JOptionPane.YES_OPTION) {
			    tableModel.removeRow(selectedRow);
			}
			
		});
		panel.add(delete);
		panel.add(new JLabel(""));
		
		// Search for a loan request using the CIN
		search.addActionListener(e ->{
			String searchCin = search_field.getText();
			if(searchCin.isEmpty()) {
				JOptionPane.showMessageDialog(frame, "Please enter a CIN to search");
				return;
			}
			boolean found = false;
			
			// Loop through the table rows to find the CIN
			for(int i = 0 ; i < tableModel.getRowCount() ; i++) {
				String tableCin = tableModel.getValueAt(i, 1).toString();
				if(tableCin.equals(searchCin)) {
					table.setRowSelectionInterval(i, i);
					found = true;
					break;
				}
				
			}
			if(!found) {
				JOptionPane.showMessageDialog(frame, "No request found with this CIN.");
			}
		});
		
		// Save all table data into a CSV file
		save.addActionListener(e -> {
		    try {
		        PrintWriter writer = new PrintWriter(new FileWriter("loan_requests.csv"));

		     // Save column names
		        for (int i = 0; i < tableModel.getColumnCount(); i++) {
		            writer.print(tableModel.getColumnName(i));

		            if (i < tableModel.getColumnCount() - 1) {
		                writer.print(",");
		            }
		        }

		        writer.println();

		     // Save table rows
		        for (int row = 0; row < tableModel.getRowCount(); row++) {
		            for (int col = 0; col < tableModel.getColumnCount(); col++) {
		                writer.print(tableModel.getValueAt(row, col));

		                if (col < tableModel.getColumnCount() - 1) {
		                    writer.print(",");
		                }
		            }

		            writer.println();
		        }

		        writer.close();

		        JOptionPane.showMessageDialog(frame, "Data saved successfully.");

		    } catch (Exception ex) {
		        JOptionPane.showMessageDialog(frame, "Error while saving data.");
		    }
		});
		
		
			
	      
		// Display the application window
		frame.setVisible(true);
		
		
	}

}
