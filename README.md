# coursework
-- Create the Geographical_Location table
CREATE TABLE Geographical_Location (
    Location_ID INT(11) PRIMARY KEY,
    Village VARCHAR(100),
    Parish VARCHAR(100),
    Sub_count VARCHAR(100),
    County VARCHAR(100),
    Region VARCHAR(50),
    Population INT(11),
    Coordinates VARCHAR(100),
    Malaria_Risk_Level VARCHAR(50),
    Health_Facilities_count INT(11),
    INT_Coverage DECIMAL(5,2),
    Report_case INT(11)
);

-- Create the Patient_data table
CREATE TABLE Patient_data (
    Patient_ID INT(11) PRIMARY KEY,
    First_Name VARCHAR(50),
    Last_Name VARCHAR(50),
    Date_of_Birth DATE,
    Gender VARCHAR(10),
    Phone_Number VARCHAR(20),
    Next_of_Kin VARCHAR(100),
    Location_ID INT(11),
    Date_Added DATE,
    Update_Date DATE,
    FOREIGN KEY (Location_ID) REFERENCES Geographical_Location(Location_ID)
);

-- Create the Supply_chain table
CREATE TABLE Supply_chain (
    Supply_ID INT(11) PRIMARY KEY,
    Resource_ID INT(11),
    Facility_ID INT(11),
    Quantity_Shipped INT(11),
    Shipment_Date DATE,
    Expected_Arrival_Date DATE,
    Shipped_by INT(11),
    Status VARCHAR(100),
    Update_Date DATE,
    FOREIGN KEY (Resource_ID) REFERENCES Resource(Resource_ID),
    FOREIGN KEY (Facility_ID) REFERENCES Health_facility(Facility_ID)
);

-- Create the Epidemiology_data table
CREATE TABLE Epidemiology_data (
    Date_ID INT(11) PRIMARY KEY,
    Location_ID INT(11),
    Recorded_Date DATE,
    Cases_per_Thousand_People INT,
    Rainfall INT(11),
    Average_Temperature DECIMAL(5,2),
    Update_Date DATE,
    Added_By INT(11),
    FOREIGN KEY (Location_ID) REFERENCES Geographical_Location(Location_ID)
);

-- Create the Training table
CREATE TABLE Training (
    Training_ID INT(11) PRIMARY KEY,
    User_ID INT(11),
    Training_Type VARCHAR(100),
    Training_Date DATE,
    Completion_Status VARCHAR(100),
    FOREIGN KEY (User_ID) REFERENCES User(User_ID)
);

-- Create the Facility_type table
CREATE TABLE Facility_type (
    Facility_type_ID INT(11) PRIMARY KEY,
    Name VARCHAR(100),
    Description TEXT,
    Date_Added DATE,
    Date_Updated DATE
);

-- Create the Treatment_outcome table
CREATE TABLE Treatment_outcome (
    Outcome_ID INT(11) PRIMARY KEY,
    Outcome_name VARCHAR(100),
    Outcome_description TEXT,
    Date_Added DATE,
    Added_By INT(11),
    Update_Date DATE
);

-- Create the Laboratory_test table
CREATE TABLE Laboratory_test (
    Test_ID INT(11) PRIMARY KEY,
    Case_ID INT(11),
    Test_Type VARCHAR(100),
    Test_Result VARCHAR(100),
    Test_date DATE,
    Technician_ID INT(11),
    FOREIGN KEY (Case_ID) REFERENCES Malaria_Cases(Case_ID)
);

-- Create the Health_facility table
CREATE TABLE Health_facility (
    Facility_ID INT(11) PRIMARY KEY,
    Facility_Name VARCHAR(100),
    Location_ID INT(11),
    Facility_Type INT(11),
    Capacity INT(11),
    Contact_Detials VARCHAR(100),
    Date_Added DATE,
    Facility_Head VARCHAR(100),
    FOREIGN KEY (Location_ID) REFERENCES Geographical_Location(Location_ID),
    FOREIGN KEY (Facility_Type) REFERENCES Facility_type(Facility_type_ID)
);

-- Create the Resource table
CREATE TABLE Resource (
    Resource_ID INT(11) PRIMARY KEY,
    Facility_ID INT(11),
    Resource_Type VARCHAR(100),
    Quantity INT(11),
    Last_Updated_Date DATE,
    Description TEXT,
    Date_Added DATE,
    Update_Date DATE,
    FOREIGN KEY (Facility_ID) REFERENCES Health_facility(Facility_ID)
);

-- Create the Treatment table
CREATE TABLE Treatment (
    Treatment_ID INT(11) PRIMARY KEY,
    Treatment_Name VARCHAR(100),
    Treatment_description TEXT,
    Dosage VARCHAR(100),
    Side_Effects TEXT,
    Date_Added DATE,
    Update_Date DATE
);

-- Create the Visit_Record table
CREATE TABLE Visit_Record (
    Visit_ID INT(11) PRIMARY KEY,
    Patient_ID INT(11),
    Visit_Number INT(11),
    Visit_Date DATE,
    Date_Added DATE,
    Update_Date DATE,
    Facility_ID INT(11),
    FOREIGN KEY (Patient_ID) REFERENCES Patient_data(Patient_ID),
    FOREIGN KEY (Facility_ID) REFERENCES Health_facility(Facility_ID)
);

-- Create the Interventions table
CREATE TABLE Interventions (
    Intervention_ID INT(11) PRIMARY KEY,
    Type VARCHAR(100),
    Location_ID INT(11),
    Start_Date DATE,
    End_Date DATE,
    Outcome VARCHAR(100),
    Date_Added DATE,
    Update_Date DATE,
    FOREIGN KEY (Location_ID) REFERENCES Geographical_Location(Location_ID)
);

-- Create the System_Log table
CREATE TABLE System_Log (
    Log_ID INT(11) PRIMARY KEY,
    User_ID INT(11),
    Activity TEXT,
    Timestamp DATETIME,
    IP_Address VARCHAR(100),
    Location VARCHAR(100),
    FOREIGN KEY (User_ID) REFERENCES User(User_ID)
);

-- Create the Malaria_Type table
CREATE TABLE Malaria_Type (
    Type_ID INT(11) PRIMARY KEY,
    Type_name VARCHAR(100),
    Description TEXT,
    Date_Added DATE,
    Added_By INT(11),
    Update_Date DATE
);

-- Create the User table
CREATE TABLE User (
    User_ID INT(11) PRIMARY KEY,
    First_Name VARCHAR(100),
    Last_Name VARCHAR(100),
    Prefered_Name VARCHAR(100),
    Role_ID INT(11),
    Username VARCHAR(100),
    Password VARCHAR(100),
    Facility_ID INT(11),
    FOREIGN KEY (Role_ID) REFERENCES User_role(Role_ID),
    FOREIGN KEY (Facility_ID) REFERENCES Health_facility(Facility_ID)
);

-- Create the Referral table
CREATE TABLE Referral (
    Referral_ID INT(11) PRIMARY KEY,
    Case_ID INT(11),
    Referral_From INT(11),
    Referred_to INT(11),
    Referral_Date DATE,
    Reason TEXT,
    Outcome_IT INT(11),
    Update_Date DATE,
    Referred_by INT(11),
    FOREIGN KEY (Case_ID) REFERENCES Malaria_Cases(Case_ID),
    FOREIGN KEY (Referral_From) REFERENCES Health_facility(Facility_ID),
    FOREIGN KEY (Referred_to) REFERENCES Health_facility(Facility_ID),
    FOREIGN KEY (Referred_by) REFERENCES User(User_ID)
);

-- Create the User_role table
CREATE TABLE User_role (
    Role_ID INT(11) PRIMARY KEY,
    Role_Name VARCHAR(100),
    Role_Description TEXT,
    Date_added DATE,
    Update_Date DATE
);

-- Create the Malaria_Cases table
CREATE TABLE Malaria_Cases (
    Case_ID INT(11) PRIMARY KEY,
    Patient_ID INT(11),
    Facility_ID INT(11),
    Date_Of_Diagnosis DATE,
    Type_Of_Malaria VARCHAR(100),
    Treatment_ID INT(11),
    Outcome_ID INT(11),
    FOREIGN KEY (Patient_ID) REFERENCES Patient_data(Patient_ID),
    FOREIGN KEY (Facility_ID) REFERENCES Health_facility(Facility_ID),
    FOREIGN KEY (Treatment_ID) REFERENCES Treatment(Treatment_ID),
    FOREIGN KEY (Outcome_ID) REFERENCES Treatment_outcome(Outcome_ID)
);

