<div align="center">

## Java Address Book Database


</div>

### Description

Simple Address Book database. Some functionality like database grid view, submit new entries, delete entries, clear fields, find entries. Used MSAccess as data "holder". Good for beginners who want to get familiar with SQL commands and jdbc/odbc. Feedback would be cool. Used JBuilder3/jdk 1.2 to develop.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Steven Jacobs](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/steven-jacobs.md)
**Level**          |Intermediate
**User Rating**    |4.6 (65 globes from 14 users)
**Compatibility**  |Java \(JDK 1\.1\)
**Category**       |[Databases/ JDBC](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/databases-jdbc__2-61.md)
**World**          |[Java](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/java.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/steven-jacobs-java-address-book-database__2-1795/archive/master.zip)





### Source Code

```
/////////////////////////////////////////////////////////////////////////////
//Name of File: AddressBook.java
//Purpose:    Input data/Find data from an Access database
//Author:    Steven Jacobs
//Date:     03/29/2000
//Comments:   Feel free to change/improve/etc. Any questions or concerns,
//        email me at sjcplus@aol.com
/////////////////////////////////////////////////////////////////////////////
package untitled20;
import java.awt.*;
import java.awt.event.*;
import java.applet.*;
import com.borland.jbcl.layout.*;
import com.borland.jbcl.control.*;
import java.sql.*;
import javax.swing.*;
import java.util.*;
public class AddressBook extends Applet
{
 FlowLayout flow = new FlowLayout(FlowLayout.LEFT);
 boolean isStandalone = false;
 Label nameLabel = new Label("Name: ");
 Label addressLabel = new Label("Address: ");
 Label emailLabel = new Label("Email: ");
 Label isConnectLabel = new Label("Connected??    ");
 TextField nameField = new TextField(40);
 TextField addressField = new TextField(40);
 TextField emailField = new TextField(40);
 TextField connectField = new TextField(50);
 Button submitButton = new Button("Submit");
 Button findButton = new Button("Find");
 Button clearButton = new Button("Clear Fields");
 Button gridButton = new Button("Show Database Grid");
 Button deleteButton = new Button("Delete Record");
 Button exitButton = new Button("Exit");
 private String url;
 private Connection connect;
 //Construct the applet
 public AddressBook()
 {
 }
 //Initialize the applet
 public void init()
 {
  try
  {
   jbInit();
  }
  catch(Exception e)
  {
   e.printStackTrace();
  }
 }
 //Component initialization
 private void jbInit() throws Exception
 {
  this.setLayout(flow);
  add(nameLabel);
  add(nameField);
  add(addressLabel);
  add(addressField);
  add(emailLabel);
  add(emailField);
  add(isConnectLabel);
  add(connectField);
  add(submitButton);
  add(findButton);
  findButton.addActionListener(new ActionListener()
   {
    public void actionPerformed(ActionEvent e)
     {
      try
       {
        Statement state = connect.createStatement();
        if (nameField.getText().equals(""))
         {
          isConnectLabel.setText("No Value");
          connectField.setText("You must enter a value in the 'Name' field");
          state.close();
         }
        else
        {
        String query = "SELECT * FROM info " +
                "WHERE Name = '" +
                nameField.getText() + "'";
        isConnectLabel.setText("Querying??");
        connectField.setText("Sending Query!!" + connect.nativeSQL(query));
        ResultSet rS = state.executeQuery(query);
        display(rS);
        connectField.setText("Successfull Query");
        state.close();
        }
       }
       catch (SQLException sqlex)
        {
         sqlex.printStackTrace();
         connectField.setText(sqlex.toString());
        }
      }
    public void display(ResultSet rS)
     {
      try
       {
        rS.next();
        nameField.setText(rS.getString(1));
        addressField.setText(rS.getString(2));
        emailField.setText(rS.getString(3));
       }
       catch (SQLException sqlX)
        {
         sqlX.printStackTrace();
         connectField.setText(sqlX.toString());
        }
      }
   });
  add(clearButton);
  clearButton.addActionListener(new ActionListener()
   {
    public void actionPerformed(ActionEvent e)
     {
      nameField.setText("");
      addressField.setText("");
      emailField.setText("");
      isConnectLabel.setText("Clearing Fields??");
      connectField.setText("Fields Cleared..You may enter a new record..");
     }
    });
  add(gridButton);
  gridButton.addActionListener(new ActionListener()
   {
    public void actionPerformed(ActionEvent e)
     {
      AddressBookGrid adg = new AddressBookGrid();
      Dimension dlgSize = adg.getSize();
      Dimension frmSize = getSize();
      Point loc = getLocation();
      adg.setLocation((frmSize.width = dlgSize.width)/2 + loc.x, (frmSize.height - dlgSize.height)/2 + loc.y);
      adg.show();
     }
   });
  add(deleteButton);
  deleteButton.addActionListener(new ActionListener()
   {
    public void actionPerformed(ActionEvent e)
     {
      try
       {
        Statement state = connect.createStatement();
        if (nameField.getText().equals(""))
         {
          isConnectLabel.setText("No Value to Delete");
          connectField.setText("You must enter a value in the 'Name' field to delete the record");
          state.close();
         }
        else
        {
        String query = "DELETE * FROM info " +
                "WHERE Name = '" +
                nameField.getText() + "'";
        isConnectLabel.setText("Deleting??");
        connectField.setText("Sending Query Delete!!" + connect.nativeSQL(query));
        state.executeQuery(query);
        connectField.setText("Successfull Deletion");
        state.close();
        }
       }
       catch (SQLException sqlex)
        {
         sqlex.printStackTrace();
         connectField.setText(sqlex.toString());
        }
      }
    });
  add(exitButton);
  exitButton.addActionListener(new ActionListener()
   {
    public void actionPerformed(ActionEvent e)
     {
      System.exit(0);
     }
   });
  submitButton.addActionListener(new ActionListener()
   {
    public void actionPerformed(ActionEvent e)
     {
      try
       {
        Statement state = connect.createStatement();
        if (nameField.getText().equals("") &&
          addressField.getText().equals("") &&
          emailField.getText().equals(""))
          {
           isConnectLabel.setText("No Values!!");
           connectField.setText("You must enter values in the field");
          }
       else
        {
        String query = "INSERT INTO info (" +
        "Name, " +
        "Address, " +
        "Email" +
        ") VALUES ('" +
        nameField.getText() + "', '" +
        addressField.getText() + "', '" +
        emailField.getText() + "')";
        connectField.setText("Sending query: " + connect.nativeSQL(query));
        int result = state.executeUpdate(query);
        if (result == 1)
         {
          isConnectLabel.setText("Submitted??");
          connectField.setText("Insertion successful");
         }
        else
         {
          isConnectLabel.setText("Submitted??");
          connectField.setText("Insertion unsuccessful");
         }
       } }
       catch (SQLException sqlex)
        {
         sqlex.printStackTrace();
         connectField.setText(sqlex.toString());
        }
    }});
  try
   {
     url = "jdbc:odbc:AddressBook";
     Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");
     connect = DriverManager.getConnection(url);
     connectField.setText("Connection successful");
   }
   catch(ClassNotFoundException cnfx)
    {
     cnfx.printStackTrace();
     connectField.setText("Connection unsuccessful" + cnfx.toString());
    }
   catch (SQLException sqlx)
    {
     sqlx.printStackTrace();
     connectField.setText("Connection unsuccessful" + sqlx.toString());
    }
   catch (Exception ex)
    {
     ex.printStackTrace();
     connectField.setText("Connection unsuccessful" + ex.toString());
    }
 }
//Get Applet information
 public String getAppletInfo()
 {
  return "Applet Information";
 }
 //Get parameter info
 public String[][] getParameterInfo()
 {
  return null;
 }
 //Main method
 public static void main(String[] args)
 {
  AddressBook applet = new AddressBook();
  applet.isStandalone = true;
  DecoratedFrame frame = new DecoratedFrame();
  frame.setTitle("Address Book Applet");
  frame.add(applet, BorderLayout.CENTER);
  applet.init();
  applet.start();
  frame.setSize(400,250);
  Dimension d = Toolkit.getDefaultToolkit().getScreenSize();
  frame.setLocation((d.width - frame.getSize().width) / 2, (d.height - frame.getSize().height) / 2);
  frame.setVisible(true);
 }
}
/////////////////////////////////////////////////
//beginning of second source file
/////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////
//Name of File: AddressBookGrid.java
//Purpose:    Shows the contents of your database in a java JTable
//Author:    Steven Jacobs
//Date:     03/29/2000
//Comments:   Feel free to change/improve/etc. Any questions or concerns,
//        email me at sjcplus@aol.com
/////////////////////////////////////////////////////////////////////////////
package untitled20;
import java.awt.*;
import javax.swing.JFrame;
import java.sql.*;
import java.awt.event.*;
import java.util.*;
import javax.swing.*;
public class AddressBookGrid extends JFrame
{
 private Connection connection;
 private JTable table;
 private Vector gridColumns;
 private Vector gridRows;
 AddressBook Ab;
 public AddressBookGrid()
 {
  String url = "jdbc:odbc:AddressBook";
  try
   {
    Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");
    connection = DriverManager.getConnection(url);
   }
   catch (ClassNotFoundException cnf)
    {
      Ab.connectField.setText("Failed to Connect to Driver");
    }
   catch (SQLException sqlx)
    {
     Ab.connectField.setText("Unable to connect");
    }
   showDatabaseTable();
   setSize(450,150);
   setTitle("Address Book Grid View");
   show();
 }
 private void showDatabaseTable()
 {
  try
   {
    Statement state = connection.createStatement();
    String query = "SELECT * FROM info";
    ResultSet result = state.executeQuery(query);
    displayDatabaseRecords(result);
    state.close();
   }
   catch (SQLException sqlx)
    {
     sqlx.printStackTrace();
    }
  }
 private void displayDatabaseRecords(ResultSet rS) throws SQLException
 {
  boolean moreRecords = rS.next();
  if (!moreRecords)
   {
    Ab.connectField.setText("No records to display");
   }
  gridColumns = new Vector();
  gridRows = new Vector();
  try
   {
    ResultSetMetaData rs = rS.getMetaData();
    for (int i = 1; i <= rs.getColumnCount(); ++i)
     gridColumns.addElement(rs.getColumnName(i));
    do
     {
      gridRows.addElement(getNextRow(rS,rs));
     }while (rS.next());
      table = new JTable (gridRows, gridColumns);
      JScrollPane scroller = new JScrollPane(table);
      getContentPane().add(scroller, BorderLayout.CENTER);
      validate();
    }
    catch(SQLException sqlx)
     {
      sqlx.printStackTrace();
     }
  }
private Vector getNextRow(ResultSet rS, ResultSetMetaData rs) throws SQLException
 {
  Vector currentRow = new Vector();
  for (int i = 1; i <= rs.getColumnCount(); ++i)
   if (rs.getColumnType(i) == Types.VARCHAR)
    {
     currentRow.addElement(rS.getString(i));
    }
   else
    {
     Ab.connectField.setText("Type was: " + rs.getColumnTypeName(i));
    }
  return currentRow;
  }
}
```

