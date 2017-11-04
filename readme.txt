JavaFX 8 Tutorial - Part 1: Scene Builder
Apr 19, 2014 • Updated Mar 12, 2015
Screenshot AddressApp Part 1

Topics in Part 1
Getting to know JavaFX
Creating and starting a JavaFX Project
Using Scene Builder to design the user interface
Basic application structure using the Model-View-Controller (MVC) pattern
Prerequisites
Latest Java JDK 8 (includes JavaFX 8).
Eclipse 4.4 or greater with e(fx)clipse plugin. The easiest way is to download the preconfigured distro from the e(fx)clipse website. As an alternative you can use an update site for your Eclipse installation.
Scene Builder 8.0 (provided by Gluon because Oracle only ships it in source code form).
Eclipse Configurations
We need to tell Eclipse to use JDK 8 and also where it will find the Scene Builder:

Open the Eclipse Preferences and navigate to Java | Installed JREs.

Click Add..., select Standard VM and choose the installation Directory of your JDK 8.

Remove the other JREs or JDKs so that the JDK 8 becomes the default.
Preferences JDK

Navigate to Java | Compiler. Set the Compiler compliance level to 1.8.
Preferences Compliance

Navigate to the JavaFX preferences. Specify the path to your Scene Builder executable.
Preferences JavaFX

Helpful Links
You might want to bookmark the following links:

Java 8 API - JavaDoc for the standard Java classes
JavaFX 8 API - JavaDoc for JavaFX classes
ControlsFX API - JavaDoc for the ControlsFX project for additional JavaFX controls
Oracle's JavaFX Tutorials - Official JavaFX Tutorials by Oracle
Now, let's get started!

Create a new JavaFX Project
In Eclipse (with e(fx)clipse installed) go to File | New | Other... and choose JavaFX Project.
Specify the Name of the project (e.g. AddressApp) and click Finish.

Remove the application package and its content if it was automatically created.

Create the Packages
Right from the start we will follow good software design principles. One very important principle is that of Model-View-Controller (MVC). According to this we divide our code into three units and create a package for each (Right-click on the src-folder, New... | Package):

ch.makery.address - contains most controller classes (=business logic)
ch.makery.address.model - contains model classes
ch.makery.address.view - contains views
Note: Our view package will also contain some controllers that are directly related to a single view. Let's call them view-controllers.

Create the FXML Layout File
There are two ways to create the user interface. Either using an XML file or programming everything in Java. Looking around the internet you will encounter both. We will use XML (ending in .fxml) for most parts. I find it a cleaner way to keep the controller and view separated from each other. Further, we can use the graphical Scene Builder to edit our XML. That means we will not have to directly work with XML.

Right-click on the view package and create a new FXML Document called PersonOverview.

New FXML Document

New PersonOverview

Design with Scene Builder
Note: If you can't get it to work, download the source of this tutorial part and try it with the included fxml.
Right-click on PersonOverview.fxml and choose Open with Scene Builder. Now you should see the Scene Builder with just an AncherPane (visible under Hierarchy on the left).

(If Scene Builder does not open, go to Window | Preferences | JavaFX and set the correct path to your Scene Builder installation).

Select the Anchor Pane in your Hierarchy and adjust the size under Layout (right side):
Anchor Pane Size

Add a Split Pane (Horizontal Flow) by dragging it from the Library into the main area. Right-click the Split Pane in the Hierarchy view and select Fit to Parent.
Fit to Parent

Drag a TableView (under Controls) into the left side of the SplitPane. Select the TableView (not a Column) and set the following layout constraints to the TableView. Inside an AnchorPane you can always set anchors to the four borders (more information on Layouts).
TableView Anchors

Go to the menu Preview | Show Preview in Window to see, whether it behaves right. Try resizing the window. The TableView should resize together with the window as it is anchored to the borders.

Change the column text (under Properties) to "First Name" and "Last Name".
Column Texts

Select the TableView and choose constrained-resize for the Column Resize Policy (under Properties). This ensures that the colums will always take up all available space.
Column Resize Policy

Add a Label on the right side with the text "Person Details" (hint: use the search to find the Label). Adjust it's layout using anchors.
Person Details Label

Add a GridPane on the right side, select it and adjust its layout using anchors (top, right and left).
GridPane Layout

Add the following labels to the cells.
Note: To add a row to the GridPane select an existing row number (will turn yellow), right-click the row number and choose "Add Row".
Add labels

Add a ButtonBar at the bottom. Add three buttons to the bar. Now, set anchors (right and bottom) to the ButtonBar so it stays in the right place.
Button Group

Now you should see something like the following. Use the Preview menu to test its resizing behaviour.
Preview

Create the Main Application
We need another FXML for our root layout which will contain a menu bar and wraps the just created PersonOverview.fxml.

Create another FXML Document inside the view package called RootLayout.fxml. This time, choose BorderPane as the root element.
New RootLayout

Open the RootLayout.fxml in Scene Builder.

Resize the BorderPane with Pref Width set to 600 and Pref Height set to 400.
RootLayout Size

Add a MenuBar into the TOP Slot. We will not implement the menu functionality at the moment.
MenuBar

The JavaFX Main Class
Now, we need to create the main java class that starts up our application with the RootLayout.fxml and adds the PersonOverview.fxml in the center.

Right-click on your project and choose New | Other... and choose JavaFX Main Class.
New JavaFX Main Class

We'll call the class MainApp and put it in the controller package ch.makery.address (note: this is the parent package of the view and model subpackages).
New JavaFX Main Class

The generated MainApp.java class extends from Application and contains two methods. This is the basic structure that we need to start a JavaFX Application. The most important part for us is the start(Stage primaryStage) method. It is automatically called when the application is launched from within the main method.

As you see, the start(...) method receives a Stage as parameter. The following graphic illustrates the structure of every JavaFX application:

New FXML Document
Image Source: http://www.oracle.com

It's like a theater play: The Stage is the main container which is usually a Window with a border and the typical minimize, maximize and close buttons. Inside the Stage you add a Scene which can, of course, be switched out by another Scene. Inside the Scene the actual JavaFX nodes like AnchorPane, TextBox, etc. are added.

For more information on this turn to Working with the JavaFX Scene Graph.

Open MainApp.java and replace the code with the following:

package ch.makery.address;

import java.io.IOException;

import javafx.application.Application;
import javafx.fxml.FXMLLoader;
import javafx.scene.Scene;
import javafx.scene.layout.AnchorPane;
import javafx.scene.layout.BorderPane;
import javafx.stage.Stage;

public class MainApp extends Application {

    private Stage primaryStage;
    private BorderPane rootLayout;

    @Override
    public void start(Stage primaryStage) {
        this.primaryStage = primaryStage;
        this.primaryStage.setTitle("AddressApp");

        initRootLayout();

        showPersonOverview();
    }

    /**
     * Initializes the root layout.
     */
    public void initRootLayout() {
        try {
            // Load root layout from fxml file.
            FXMLLoader loader = new FXMLLoader();
            loader.setLocation(MainApp.class.getResource("view/RootLayout.fxml"));
            rootLayout = (BorderPane) loader.load();

            // Show the scene containing the root layout.
            Scene scene = new Scene(rootLayout);
            primaryStage.setScene(scene);
            primaryStage.show();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * Shows the person overview inside the root layout.
     */
    public void showPersonOverview() {
        try {
            // Load person overview.
            FXMLLoader loader = new FXMLLoader();
            loader.setLocation(MainApp.class.getResource("view/PersonOverview.fxml"));
            AnchorPane personOverview = (AnchorPane) loader.load();

            // Set person overview into the center of root layout.
            rootLayout.setCenter(personOverview);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * Returns the main stage.
     * @return
     */
    public Stage getPrimaryStage() {
        return primaryStage;
    }

    public static void main(String[] args) {
        launch(args);
    }
}
The various comments should give you some hints about what's going on.

If you run the application now, you should see something like the screenshot at the beginning of this post.

Frequent Problems
If JavaFX can't find the fxml file you specified, you might get the following error message:

java.lang.IllegalStateException: Location is not set.

To solve this issue double check if you didn't misspell the name of your fxml files!

If it still doesn't work, download the source of this tutorial part and try it with the included fxml.

JavaFX 8 Tutorial - Part 2: Model and TableView
Apr 23, 2014 • Updated Mar 12, 2015
Screenshot AddressApp Part 2

Topics in Part 2
Creating a model class
Using the model class in an ObservableList
Show data in the TableView using Controllers
Create the Model Class
We need a model class to hold information about the people in our address book. Add a new class to the model package (ch.makery.address.model) called Person. The Person class will have a few instance variables for the name, address and birthday. Add the following code to the class. I'll explain some JavaFX specifics after the code.

Person.java

package ch.makery.address.model;

import java.time.LocalDate;

import javafx.beans.property.IntegerProperty;
import javafx.beans.property.ObjectProperty;
import javafx.beans.property.SimpleIntegerProperty;
import javafx.beans.property.SimpleObjectProperty;
import javafx.beans.property.SimpleStringProperty;
import javafx.beans.property.StringProperty;

/**
 * Model class for a Person.
 *
 * @author Marco Jakob
 */
public class Person {

    private final StringProperty firstName;
    private final StringProperty lastName;
    private final StringProperty street;
    private final IntegerProperty postalCode;
    private final StringProperty city;
    private final ObjectProperty<LocalDate> birthday;

    /**
     * Default constructor.
     */
    public Person() {
        this(null, null);
    }

    /**
     * Constructor with some initial data.
     * 
     * @param firstName
     * @param lastName
     */
    public Person(String firstName, String lastName) {
        this.firstName = new SimpleStringProperty(firstName);
        this.lastName = new SimpleStringProperty(lastName);

        // Some initial dummy data, just for convenient testing.
        this.street = new SimpleStringProperty("some street");
        this.postalCode = new SimpleIntegerProperty(1234);
        this.city = new SimpleStringProperty("some city");
        this.birthday = new SimpleObjectProperty<LocalDate>(LocalDate.of(1999, 2, 21));
    }

    public String getFirstName() {
        return firstName.get();
    }

    public void setFirstName(String firstName) {
        this.firstName.set(firstName);
    }

    public StringProperty firstNameProperty() {
        return firstName;
    }

    public String getLastName() {
        return lastName.get();
    }

    public void setLastName(String lastName) {
        this.lastName.set(lastName);
    }

    public StringProperty lastNameProperty() {
        return lastName;
    }

    public String getStreet() {
        return street.get();
    }

    public void setStreet(String street) {
        this.street.set(street);
    }

    public StringProperty streetProperty() {
        return street;
    }

    public int getPostalCode() {
        return postalCode.get();
    }

    public void setPostalCode(int postalCode) {
        this.postalCode.set(postalCode);
    }

    public IntegerProperty postalCodeProperty() {
        return postalCode;
    }

    public String getCity() {
        return city.get();
    }

    public void setCity(String city) {
        this.city.set(city);
    }

    public StringProperty cityProperty() {
        return city;
    }

    public LocalDate getBirthday() {
        return birthday.get();
    }

    public void setBirthday(LocalDate birthday) {
        this.birthday.set(birthday);
    }

    public ObjectProperty<LocalDate> birthdayProperty() {
        return birthday;
    }
}
Explanations
With JavaFX it's common to use Properties for all fields of a model class. A Property allows us, for example, to automatically be notified when the lastName or any other variable is changed. This helps us keep the view in sync with the data. To learn more about Properties read Using JavaFX Properties and Binding.
LocalDate, the type we're using for birthday, is part of the new Date and Time API for JDK 8.
A List of Persons
The main Data that our application manages, is a bunch of persons. Let's create a list for Person objects inside the MainApp class. All other controller classes will later get access to that central list inside the MainApp.

ObservableList
We are working with JavaFX view classes that need to be informed about any changes made to the list of persons. This is important, since otherwise the view would not be in sync with the data. For this purpose, JavaFX introduces some new Collection classes.

From those collections, we need the ObservableList. To create a new ObservableList, add the following code at the beginning of the MainApp class. We'll also add a constructor that creates some sample data and a public getter method:

MainApp.java

    // ... AFTER THE OTHER VARIABLES ...

    /**
     * The data as an observable list of Persons.
     */
    private ObservableList<Person> personData = FXCollections.observableArrayList();

    /**
     * Constructor
     */
    public MainApp() {
        // Add some sample data
        personData.add(new Person("Hans", "Muster"));
        personData.add(new Person("Ruth", "Mueller"));
        personData.add(new Person("Heinz", "Kurz"));
        personData.add(new Person("Cornelia", "Meier"));
        personData.add(new Person("Werner", "Meyer"));
        personData.add(new Person("Lydia", "Kunz"));
        personData.add(new Person("Anna", "Best"));
        personData.add(new Person("Stefan", "Meier"));
        personData.add(new Person("Martin", "Mueller"));
    }

    /**
     * Returns the data as an observable list of Persons. 
     * @return
     */
    public ObservableList<Person> getPersonData() {
        return personData;
    }

    // ... THE REST OF THE CLASS ...
The PersonOverviewController
Now let's finally get some data into our table. We'll need a controller for our PersonOverview.fxml.

Create a normal class inside the view package called PersonOverviewController.java. (We must put it in the same package as the PersonOverview.fxml, otherwise the SceneBuilder won't find it.)
We'll add some instance variables that give us access to the table and the labels inside the view. The fields and some methods have a special @FXML annotation. This is necessary for the fxml file to have access to private fields and private methods. After we have everything set up in the fxml file, the application will automatically fill the variables when the fxml file is loaded. So let's add the following code:
Note: Remember to always use the javafx imports, NOT awt or swing!
PersonOverviewController.java

package ch.makery.address.view;

import javafx.fxml.FXML;
import javafx.scene.control.Label;
import javafx.scene.control.TableColumn;
import javafx.scene.control.TableView;
import ch.makery.address.MainApp;
import ch.makery.address.model.Person;

public class PersonOverviewController {
    @FXML
    private TableView<Person> personTable;
    @FXML
    private TableColumn<Person, String> firstNameColumn;
    @FXML
    private TableColumn<Person, String> lastNameColumn;

    @FXML
    private Label firstNameLabel;
    @FXML
    private Label lastNameLabel;
    @FXML
    private Label streetLabel;
    @FXML
    private Label postalCodeLabel;
    @FXML
    private Label cityLabel;
    @FXML
    private Label birthdayLabel;

    // Reference to the main application.
    private MainApp mainApp;

    /**
     * The constructor.
     * The constructor is called before the initialize() method.
     */
    public PersonOverviewController() {
    }

    /**
     * Initializes the controller class. This method is automatically called
     * after the fxml file has been loaded.
     */
    @FXML
    private void initialize() {
        // Initialize the person table with the two columns.
        firstNameColumn.setCellValueFactory(cellData -> cellData.getValue().firstNameProperty());
        lastNameColumn.setCellValueFactory(cellData -> cellData.getValue().lastNameProperty());
    }

    /**
     * Is called by the main application to give a reference back to itself.
     * 
     * @param mainApp
     */
    public void setMainApp(MainApp mainApp) {
        this.mainApp = mainApp;

        // Add observable list data to the table
        personTable.setItems(mainApp.getPersonData());
    }
}
Now this code will probably need some explaining:

All fields and methods where the fxml file needs access must be annotated with @FXML. Actually, only if they are private, but it's better to have them private and mark them with the annotation!
The initialize() method is automatically called after the fxml file has been loaded. At this time, all the FXML fields should have been initialized already.
The setCellValueFactory(...) that we set on the table colums are used to determine which field inside the Person objects should be used for the particular column. The arrow -> indicates that we're using a Java 8 feature called Lambdas. (Another option would be to use a PropertyValueFactory, but this is not type-safe).
We're only using StringProperty values for our table columns in this example. When you want to use IntegerProperty or DoubleProperty, the setCellValueFactory(...) must have an additional asObject():
myIntegerColumn.setCellValueFactory(cellData -> 
      cellData.getValue().myIntegerProperty().asObject());
This is necessary because of a bad design decision of JavaFX (see this discussion).
Connecting MainApp with the PersonOverviewController
The setMainApp(...) method must be called by the MainApp class. This gives us a way to access the MainApp object and get the list of Persons and other things. Replace the showPersonOverview() method with the following. It contains two additional lines:

MainApp.java - new showPersonOverview() method

/**
 * Shows the person overview inside the root layout.
 */
public void showPersonOverview() {
    try {
        // Load person overview.
        FXMLLoader loader = new FXMLLoader();
        loader.setLocation(MainApp.class.getResource("view/PersonOverview.fxml"));
        AnchorPane personOverview = (AnchorPane) loader.load();

        // Set person overview into the center of root layout.
        rootLayout.setCenter(personOverview);

        // Give the controller access to the main app.
        PersonOverviewController controller = loader.getController();
        controller.setMainApp(this);

    } catch (IOException e) {
        e.printStackTrace();
    }
}
Hook the View to the Controller
We're almost there! But one little thing is missing: We haven't told our PersonOverview.fxml file yet, which controller to use and which element should match to which field inside the controller.

Open PersonOverview.fxml with the SceneBuilder.

Open the Controller group on the left side and select the PersonOverviewController as controller class.
Set Controller Class

Select the TableView in the Hierarchy group and choose in the Code group the personTable field as fx:id.
Set TableView fx:id

Do the same for the columns and select firstNameColumn and lastNameColumn as fx:id respectively.

For each label in the second column, choose the corresponding fx:id.
Set Label fx:id

Important: Go back to Eclipse and refresh the entire AddressApp project (F5). This is necessary because Eclipse sometimes doesn't know about changes that were made inside the Scene Builder.

Start the Application
When you start your application now, you should see something like the screenshot at the beginning of this blog post.

Congratulations!

Note: The labels will not update when a person is selected, yet. We will program user interactions in the next part of the tutorial.

What's Next?
In Tutorial Part 3 we will add more functionality like adding, deleting and editing Persons.

Some other articles you might find interesting

JavaFX Dialogs (official)
JavaFX Date Picker
JavaFX Event Handling Examples
JavaFX TableView Sorting and Filtering
JavaFX TableView Cell Renderer
