# Hospital-Management-System

CREATE SCHEMA IF NOT EXISTS hospital_management;
DROP TABLE IF EXISTS hospital_management.patient;
CREATE TABLE IF NOT EXISTS hospital_management.patient (
    email VARCHAR(50) PRIMARY KEY,
    password varchar(30) NOT NULL,
    name VARCHAR(50) NOT NULL,
    address varchar(60) NOT NULL,
    gender VARCHAR(20) NOT NULL
);

CREATE TABLE IF NOT EXISTS hospital_management.medical_history (
    medical_history_id int PRIMARY KEY,
    date DATE NOT NULL,
    conditions VARCHAR(100) NOT NULL,
    surgeries VARCHAR(100) NOT NULL,
    medication VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS hospital_management.doctor (
    email VARCHAR(50) PRIMARY KEY,
    gender varchar(20) NOT NULL,
    password varchar(30) NOT NULL,
    name VARCHAR(50) NOT NULL
);

CREATE TABLE IF NOT EXISTS hospital_management.appointment (
    appointment_id int PRIMARY KEY,
    date DATE NOT NULL,
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,
    status varchar(15) NOT NULL
);

CREATE TABLE IF NOT EXISTS hospital_management.patient_visits (
    patient VARCHAR(50) NOT NULL,
    appt INT NOT NULL,  -- Use INT, NOT SERIAL
    concerns VARCHAR(40) NOT NULL,
    symptoms VARCHAR(40) NOT NULL,
    FOREIGN KEY (patient) REFERENCES hospital_management.patient (email),
    FOREIGN KEY (appt) REFERENCES hospital_management.appointment (appointment_id),
    PRIMARY KEY (patient, appt)
);

CREATE TABLE IF NOT EXISTS hospital_management.schedule (
    schedule_id SERIAL UNIQUE,
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,
    break_time TIME NOT NULL,
    day varchar(20) NOT NULL,
    PRIMARY KEY (schedule_id, start_time, end_time, break_time, day)
);

CREATE TABLE IF NOT EXISTS hospital_management.patients_history (
    patient VARCHAR(50) NOT NULL,
    history INT NOT NULL,  -- Changed from SERIAL to INT
    FOREIGN KEY (patient) REFERENCES hospital_management.patient (email),
    FOREIGN KEY (history) REFERENCES hospital_management.medical_history (medical_history_id),
    PRIMARY KEY (history)
);

CREATE TABLE IF NOT EXISTS hospital_management.diagnose (
    appt INT NOT NULL,              -- Changed SERIAL to INT NOT NULL
    doctor VARCHAR(50) NOT NULL,
    diagnosis VARCHAR(40) NOT NULL,
    prescription VARCHAR(50) NOT NULL,
    FOREIGN KEY (appt) REFERENCES hospital_management.appointment (appointment_id),
    FOREIGN KEY (doctor) REFERENCES hospital_management.doctor (email),
    PRIMARY KEY (appt, doctor)
);

CREATE TABLE IF NOT EXISTS hospital_management.doctor_schedules (
    sched SERIAL,
    doctor VARCHAR(50) NOT NULL,
    FOREIGN KEY (sched) REFERENCES hospital_management.schedule (schedule_id),
    FOREIGN KEY (doctor) REFERENCES hospital_management.doctor (email),
    PRIMARY KEY (sched, doctor)
);

CREATE TABLE IF NOT EXISTS hospital_management.doctor_view_history (
    history INT NOT NULL,         -- Changed from SERIAL to INT NOT NULL
    doctor VARCHAR(50) NOT NULL,
    FOREIGN KEY (doctor) REFERENCES hospital_management.doctor (email),
    FOREIGN KEY (history) REFERENCES hospital_management.medical_history (medical_history_id),
    PRIMARY KEY (history, doctor)
);

