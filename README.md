
OOP (java) project for 2nd year first semester

this database worked fine. 

but im not sure anta's database was changed later to work with my crud operations (movie , admin , tv series)(changed data types to varchar)

here's the mysqlworkbench database queries and sample data


-- cinemafy
-- Create Users Table
CREATE TABLE Users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    registration_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    user_role ENUM('guest', 'registered_user', 'admin') DEFAULT 'registered_user'
);

-- Create Admins Table
CREATE TABLE Admins (
    admin_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Create Movies Table
CREATE TABLE Movies (
    movie_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    release_date DATE,
    description TEXT,
    rating DECIMAL(3, 2),
    duration INT, -- duration in minutes
    language VARCHAR(50),
    poster_image_url VARCHAR(255),
    trailer_url VARCHAR(255)
);

-- Create TV Series Table
CREATE TABLE TVSeries (
    series_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    release_date DATE,
    description TEXT,
    rating DECIMAL(3, 2),
    total_seasons INT,
    poster_image_url VARCHAR(255),
    trailer_url VARCHAR(255)
);

-- Create Reviews Table
CREATE TABLE Reviews (
    review_id INT AUTO_INCREMENT PRIMARY KEY,
    userName VARCHAR(50) NOT NULL,
    movie_tvshowName VARCHAR(100) NOT NULL,
    review_text TEXT NOT NULL,
    rating INT CHECK (rating >= 1 AND rating <= 5),
    review_date DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Create Payments Table
CREATE TABLE Payments (
    payment_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    amount DECIMAL(10, 2) NOT NULL,
    payment_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    payment_status ENUM('completed', 'pending', 'failed') DEFAULT 'pending',
    transaction_id VARCHAR(100),
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

-- Create WatchLater Table
CREATE TABLE WatchLater (
    watch_later_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    movie_id INT,
    series_id INT,
    added_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (movie_id) REFERENCES Movies(movie_id) ON DELETE CASCADE,
    FOREIGN KEY (series_id) REFERENCES TVSeries(series_id) ON DELETE CASCADE
);

-- Create Reports Table
CREATE TABLE Reports (
    report_id INT AUTO_INCREMENT PRIMARY KEY,
    admin_id INT,
    report_type ENUM('revenue_report', 'review_report') NOT NULL,
    generated_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    report_data TEXT,
    FOREIGN KEY (admin_id) REFERENCES Admins(admin_id) ON DELETE CASCADE
);





-- Insert into Users Table
INSERT INTO Users (name, username, email, password, user_role) 
VALUES 
('John Doe', 'johndoe', 'johndoe@example.com', 'hashedpassword123', 'registered_user'),
('Jane Smith', 'janesmith', 'janesmith@example.com', 'hashedpassword456', 'admin'),
('Alice Johnson', 'alicej', 'alicej@example.com', 'hashedpassword789', 'guest');

-- Insert into Admins Table
INSERT INTO Admins (username, email, password) 
VALUES 
('admin_john', 'adminjohn@example.com', 'hashedpassword789'),
('admin_jane', 'adminjane@example.com', 'hashedpassword012'),
('admin_paul', 'adminpaul@example.com', 'hashedpassword345');

-- Insert into Movies Table
INSERT INTO Movies (title, release_date, description, rating, duration, language, poster_image_url, trailer_url)
VALUES 
('Inception', '2010-07-16', 'A mind-bending thriller.', 8.8, 148, 'English', 'inception.jpg', 'https://trailer.url/inception'),
('The Matrix', '1999-03-31', 'A hacker uncovers a dystopian reality.', 8.7, 136, 'English', 'matrix.jpg', 'https://trailer.url/matrix'),
('Interstellar', '2014-11-07', 'A space exploration epic.', 8.6, 169, 'English', 'interstellar.jpg', 'https://trailer.url/interstellar'),
('The Dark Knight', '2008-07-18', 'Batman faces the Joker.', 9.0, 152, 'English', 'dark_knight.jpg', 'https://trailer.url/darkknight'),
('Pulp Fiction', '1994-10-14', 'A series of interconnected crime stories.', 8.9, 154, 'English', 'pulp_fiction.jpg', 'https://trailer.url/pulpfiction'),
('Avengers: Endgame', '2019-04-26', 'The Avengers fight Thanos.', 8.4, 181, 'English', 'endgame.jpg', 'https://trailer.url/endgame'),
('Titanic', '1997-12-19', 'A romance aboard the doomed ship.', 7.8, 195, 'English', 'titanic.jpg', 'https://trailer.url/titanic'),
('The Lion King', '1994-06-15', 'A young lion prince flees his kingdom.', 8.5, 88, 'English', 'lion_king.jpg', 'https://trailer.url/lionking'),
('Gladiator', '2000-05-05', 'A general turned gladiator seeks revenge.', 8.5, 155, 'English', 'gladiator.jpg', 'https://trailer.url/gladiator'),
('The Godfather', '1972-03-24', 'The rise of a mafia family.', 9.2, 175, 'English', 'godfather.jpg', 'https://trailer.url/godfather');

-- Insert into TVSeries Table
INSERT INTO TVSeries (title, release_date, description, rating, total_seasons, poster_image_url, trailer_url)
VALUES 
('Breaking Bad', '2008-01-20', 'A chemistry teacher turned meth producer.', 9.5, 5, 'breakingbad.jpg', 'https://trailer.url/breakingbad'),
('Stranger Things', '2016-07-15', 'A group of kids face supernatural threats.', 8.7, 4, 'strangerthings.jpg', 'https://trailer.url/strangerthings'),
('Game of Thrones', '2011-04-17', 'Noble families vie for control of Westeros.', 9.3, 8, 'got.jpg', 'https://trailer.url/got'),
('The Mandalorian', '2019-11-12', 'A lone bounty hunter navigates the galaxy.', 8.8, 3, 'mandalorian.jpg', 'https://trailer.url/mandalorian'),
('The Witcher', '2019-12-20', 'A monster hunter journeys through a fantasy world.', 8.2, 3, 'witcher.jpg', 'https://trailer.url/witcher'),
('Friends', '1994-09-22', 'Six friends navigate life in New York.', 8.9, 10, 'friends.jpg', 'https://trailer.url/friends'),
('The Office', '2005-03-24', 'A mockumentary about office employees.', 8.9, 9, 'office.jpg', 'https://trailer.url/office'),
('Sherlock', '2010-07-25', 'A modern adaptation of Sherlock Holmes.', 9.1, 4, 'sherlock.jpg', 'https://trailer.url/sherlock'),
('The Crown', '2016-11-04', 'The reign of Queen Elizabeth II.', 8.7, 6, 'crown.jpg', 'https://trailer.url/crown'),
('Peaky Blinders', '2013-09-12', 'A gang leader rises in post-WWI England.', 8.8, 6, 'peakyblinders.jpg', 'https://trailer.url/peakyblinders');

-- Insert into Reviews Table
INSERT INTO Reviews (userName, movie_tvshowName, review_text, rating) 
VALUES 
('johndoe', 'Inception', 'Amazing movie with a complex plot.', 5),
('janesmith', 'Stranger Things', 'Best TV series ever!', 5),
('alicej', 'Interstellar', 'A beautiful exploration of space and time.', 4);

-- Insert into Payments Table
INSERT INTO Payments (user_id, amount, payment_status, transaction_id) 
VALUES 
(1, 19.99, 'completed', 'TXN123456789'),
(2, 29.99, 'pending', 'TXN987654321'),
(3, 49.99, 'failed', 'TXN1122334455');

-- Insert into WatchLater Table
INSERT INTO WatchLater (user_id, movie_id, series_id) 
VALUES 
(1, 1, NULL),
(2, NULL, 1),
(3, 3, NULL);

-- Insert into Reports Table
INSERT INTO Reports (admin_id, report_type, report_data) 
VALUES 
(1, 'revenue_report', 'Revenue report data for Q3 2024'),
(2, 'review_report', 'Review data from October 2024'),
(3, 'revenue_report', 'Revenue report for September 2024');
