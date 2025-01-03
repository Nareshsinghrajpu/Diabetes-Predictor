# Diabetes-Predictor
# Created by Naresh Singh And Mitesh Jangid 

# Diabetes Predictor

# Diabetes Predictor Using Machine Learning

This is an interactive web application developed in R using **Shiny** to predict the likelihood of diabetes based on user-provided health data. It employs machine learning models for accurate and reliable predictions. 

---

## Features

- **User Authentication**: Includes signup and login functionalities with password encryption (SHA-256).
- **Data Visualization**: Provides visual insights using interactive plots (e.g., scatter plots, bar charts, and pie charts).
- **Machine Learning Models**: Uses a pre-trained Random Forest model for diabetes risk prediction.
- **Interactive UI**: Built with modern and responsive design using Shiny and custom CSS.- **Dark Mode**: Integrated dark mode toggle for a better user experience.

---

## Dependencies

Ensure the following dependencies are installed before running the application:

- **R (Version 4.0 or higher)**  
- **R Libraries**:
  - `shiny` (for building the web app)
  - `shinydashboard` (for structured UI components)
  - `shinyjs` (for interactive JS elements)
  - `digest` (for password encryption)
  - `randomForest` (for machine learning)
  - `ggplot2` and `plotly` (for data visualization)
  - `treemap` (for tree map visualization)
  - `shinyWidgets` (for advanced UI components)
  - `waiter` (for loading animations)

Install all required libraries in R:
```R
install.packages(c(
  "shiny", "shinydashboard", "shinyjs", "digest", 
  "randomForest", "ggplot2", "plotly", "treemap", 
  "shinyWidgets", "waiter"
))

How to Run the Project
Clone the Repository
Clone this repository to your local machine:


Copy code
git clone https://github.com/yourusername/diabetes-predictor.git
cd diabetes-predictor

cd diabetes-predictor
Prepare the Data
Place the cleaned_data_diabetes.csv file in the project directory. Ensure it contains the required columns:

Pregnancies, Glucose, BloodPressure, SkinThickness, Insulin, BMI, DiabetesPedigreeFunction, Age, Outcome.
Launch the App
Open the app.R file in RStudio or your preferred IDE, and run the following command in the R console:

R
Copy code
shiny::runApp("app.R")

Usage
Sign Up: Register with your username, email, and password.
Log In: Enter your credentials to access the application.
Input Data: Provide health data (e.g., glucose levels, BMI, etc.) in the "Prediction" tab to predict diabetes risk.
Visualize Data: Explore the dataset with various interactive plots in the "Visualization" tab.
View Dashboard: Summarize insights with visualizations on the "Dashboard" tab.




# Diabetes Prediction Power BI Dashboard

This project features a **Power BI Dashboard** designed to visualize and analyze diabetes-related data. It provides an interactive platform for exploring trends, distributions, and insights to assist in understanding diabetes prevalence and associated factors.

---

## Features

- **Interactive Visualizations**:
  - Age vs. Glucose level scatter plot.
  - Outcome distribution (diabetic vs. non-diabetic).
  - BMI trends across different age groups.
  - Correlation between Glucose levels and other attributes.
- **Dynamic Filters**:
  - Filter by age groups, BMI ranges, or diabetes outcomes.
- **Key Performance Indicators (KPIs)**:
  - Average Glucose levels.
  - Proportion of diabetic vs. non-diabetic cases.
  - Trends in diabetes prevalence over time (if temporal data is available).

---

## Dependencies

### Software
- **Power BI Desktop**: Download the latest version from [Microsoft Power BI](https://powerbi.microsoft.com/).

### Data Requirements
The dashboard requires the `cleaned_data_diabetes.csv` file with the following columns:
- `Pregnancies`, `Glucose`, `BloodPressure`, `SkinThickness`, `Insulin`, `BMI`, `DiabetesPedigreeFunction`, `Age`, `Outcome`.

---

## How to Use the Dashboard

1. **Clone the Repository**  
   Clone the repository to your local machine:
   ```bash
   git clone https://github.com/yourusername/diabetes-predictor.git
   cd diabetes-predictor



#  app   code

library(shiny)
library(shinydashboard)
library(shinyjs)
library(digest)
library(randomForest)
library(ggplot2)
library(plotly)
library(treemap)
library(shinyWidgets)  # For modern UI elements
library(waiter)  # For loading animations

# Initialize user data file
user_file <- "user_data.csv"
if (!file.exists(user_file)) {
  write.table(
    data.frame(Username = character(), Email = character(), Mobile = character(), Password = character()), 
    user_file, sep = ",", row.names = FALSE, col.names = TRUE
  )
}

# Authentication functions
save_user_data <- function(username, email, mobile, password) {
  hashed_password <- digest(password, algo = "sha256")
  user_data <- data.frame(
    Username = username, 
    Email = email, 
    Mobile = mobile, 
    Password = hashed_password, 
    stringsAsFactors = FALSE
  )
  write.table(user_data, user_file, sep = ",", row.names = FALSE, col.names = FALSE, append = TRUE)
}

email_exists <- function(email) {
  if (file.exists(user_file)) {
    existing_data <- read.csv(user_file, stringsAsFactors = FALSE)
    return(email %in% existing_data$Email)
  }
  return(FALSE)
}

validate_login <- function(username, password) {
  if (file.exists(user_file)) {
    user_data <- read.csv(user_file, stringsAsFactors = FALSE)
    user_row <- user_data[user_data$Username == username, ]
    if (nrow(user_row) == 1) {
      if (digest(password, algo = "sha256") == user_row$Password) {
        return(TRUE)
      }
    }
  }
  return(FALSE)
}

# Load and prepare diabetes data
diabetes_data <- read.csv("cleaned_data_diabetes.csv")
set.seed(123)
split <- sample(1:nrow(diabetes_data), 0.8 * nrow(diabetes_data))
train_data <- diabetes_data[split, ]
test_data <- diabetes_data[-split, ]

# Train Random Forest model
model <- randomForest(
  Outcome ~ Pregnancies + Glucose + BloodPressure + SkinThickness + 
    Insulin + BMI + DiabetesPedigreeFunction + Age,
  data = train_data
)

# Common CSS styles
common_styles <- HTML("
  @import url('https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&display=swap');
  
  body {
    background: linear-gradient(145deg, #0a0a0a, #1b1b1b);
    color: #ffffff;
    font-family: 'Montserrat', sans-serif;
    margin: 0;
    padding: 0;
  }
  
  .app-name {
    font-family: ' Georgia', 'serif';
    font-size: 3em;
    color: #00aaff;
    position: absolute;
    top: 20px;
    left: 20px;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
    transition: transform 0.3s ease;
  }
  
  .app-name:hover {
    transform: scale(1.05);
  }
  
  .auth-container {
    max-width: 400px;
    margin: 10% auto;
    padding: 40px;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 12px;
    backdrop-filter: blur(10px);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
    text-align: center;
    color: #ffffff;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }
  
  .auth-container:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 40px rgba(0, 0, 0, 0.4);
  }
  
  .auth-container h2 {
    font-size: 2.5em;
    font-weight: bold;
    color: #00aaff;
    margin-bottom: 30px;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
  }
  
  .auth-container input[type='text'],
  .auth-container input[type='password'] {
    width: 100%;
    padding: 12px;
    margin: 10px 0;
    border: 1px solid rgba(255, 255, 255, 0.2);
    background: rgba(0, 0, 0, 0.5);
    color: #ffffff;
    border-radius: 8px;
    outline: none;
    transition: all 0.3s ease;
  }
  
  .auth-container input[type='text']:focus,
  .auth-container input[type='password']:focus {
    border-color: #00aaff;
    box-shadow: 0 0 8px rgba(0, 170, 255, 0.4);
  }
  
  .auth-btn {
    width: 100%;
    padding: 14px;
    margin-top: 20px;
    background: #00aaff;
    color: #ffffff;
    border: none;
    border-radius: 8px;
    font-size: 1.1em;
    cursor: pointer;
    transition: all 0.3s ease;
    text-transform: uppercase;
    letter-spacing: 1px;
  }
  
  .auth-btn:hover {
    background: #0088cc;
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 170, 255, 0.3);
  }
  
  .auth-link {
    color: #00aaff;
    margin-top: 20px;
    text-decoration: none;
    transition: all 0.3s ease;
  }
  
  .auth-link:hover {
    color: #0088cc;
    text-decoration: underline;
  }
  
  .loading-screen {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
  }
  
  .loading-spinner {
    width: 50px;
    height: 50px;
    border: 5px solid #f3f3f3;
    border-top: 5px solid #00aaff;
    border-radius: 50%;
    animation: spin 1s linear infinite;
  }
  
  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  /* Previous code remains exactly the same until the modal styles section */

  /* Custom modal styles for success messages */
  .modal-content {
    background-color: #1a1a1a !important;
    border: 2px solid  #00aaff !important;
  }

  .modal-header {
    background-color: #00aaff !important;
    color: white !important;
    border-bottom: none !important;
  }

  .modal-body {
    color: #007bff !important;
    font-weight: bold !important;
  }

  .modal-footer {
    border-top: none !important;
  }

/* Rest of the code remains exactly the same */
")

# UI Components
loginUI <- function() {
  fluidPage(
    tags$head(
      tags$style(common_styles)
    ),
    useWaiter(),
    div(class = "app-name", "Diabetes Predictor"),
    div(class = "auth-container",
        tags$h2("Log In"),
        textInput("username", label = NULL, placeholder = "Username"),
        passwordInput("password", label = NULL, placeholder = "Password"),
        actionButton("login", "Log In", class = "auth-btn"),
        tags$p(
          "Not registered yet? ",
          actionLink("goto_signup", "Sign Up here", class = "auth-link")
        )
    )
  )
}

signupUI <- function() {
  fluidPage(
    tags$head(
      tags$style(common_styles)
    ),
    useWaiter(),
    div(class = "app-name", "Diabetes Predictor"),
    div(class = "auth-container",
        tags$h2("Sign Up"),
        textInput("signup_username", label = NULL, placeholder = "Username"),
        textInput("signup_email", label = NULL, placeholder = "Email"),
        textInput("signup_mobile", label = NULL, placeholder = "Mobile Number"),
        passwordInput("signup_password", label = NULL, placeholder = "Password"),
        passwordInput("signup_confirm_password", label = NULL, placeholder = "Confirm Password"),
        actionButton("signup", "Create Account", class = "auth-btn"),
        tags$p(
          "Already registered? ",
          actionLink("goto_login", "Login here", class = "auth-link")
        )
    )
  )
}

mainUI <- function() {
  dashboardPage(
    dashboardHeader(title = "Diabetes Prediction Dashboard"),
    dashboardSidebar(
      sidebarMenu(
        menuItem("Home", tabName = "home", icon = icon("home")),
        menuItem("Prediction", tabName = "prediction", icon = icon("stethoscope")),
        menuItem("Visualization", tabName = "visualization", icon = icon("chart-bar")),
        menuItem("Dashboard", tabName = "dashboard", icon = icon("dashboard")),
        menuItem("Logout", tabName = "logout", icon = icon("sign-out-alt"))
      )
    ),
    dashboardBody(
      tags$head(
        tags$style(HTML("
          .content-wrapper, .right-side {
            background-color: #2b2b2b;
            color: white;
          }
          .box {
            background-color: #333;
            color: white;
            border-top: 3px solid #3c8dbc;
            transition: transform 0.3s ease;
          }
          .box:hover {
            transform: translateY(-5px);
          }
          .btn-primary {
            background-color: #00aaff;
            border: none;
            transition: all 0.3s ease;
          }
          .btn-primary:hover {
            background-color: #0088cc;
            transform: translateY(-2px);
          }
        "))
      ),
      useWaiter(),
      tabItems(
        tabItem(
          tabName = "home",
          h2("Welcome to the Diabetes Prediction App!", class = "animated fadeIn"),
          h3("Developed By Naresh Singh And Mitesh Jangid"),
          div(
            class = "animated fadeIn",
            style = "animation-delay: 0.5s",
            p("This app uses machine learning to help predict the likelihood of diabetes based on user-provided health data."),
            materialSwitch("dark_mode", "Dark Mode", status = "primary")
          )
        ),
        tabItem(
          tabName = "prediction",
          h2("Diabetes Risk Prediction"),
          fluidRow(
            column(4, numericInput("Pregnancies", "Number of Pregnancies:", 1, 0, 20)),
            column(4, numericInput("Glucose", "Glucose Level:", 120, 50, 300)),
            column(4, numericInput("BloodPressure", "Blood Pressure:", 80, 50, 200)),
            column(4, numericInput("SkinThickness", "Skin Thickness (mm):", 20, 0, 100)),
            column(4, numericInput("Insulin", "Insulin Level:", 85, 0, 500)),
            column(4, numericInput("BMI", "Body Mass Index (BMI):", 25, 10, 50)),
            column(4, numericInput("DiabetesPedigreeFunction", "Diabetes Pedigree Function:", 0.5, 0, 2.5)),
            column(4, numericInput("Age", "Age:", 30, 1, 100))
          ),
          actionButton("predict", "Predict", class = "btn btn-primary"),
          h4("Prediction Result:"),
          textOutput("prediction_result")
        ),
        tabItem(
          tabName = "visualization",
          h2("Data Visualization"),
          fluidRow(
            column(6, selectInput("x_var", "Choose X-Axis Variable:", 
                                  choices = names(diabetes_data)[-9])),
            column(6, selectInput("y_var", "Choose Y-Axis Variable:", 
                                  choices = names(diabetes_data)[-9]))
          ),
          plotlyOutput("visualization_plot")
        ),
        tabItem(
          tabName = "dashboard",
          h2("Dashboard"),
          fluidRow(
            box(plotlyOutput("line_chart"), title = "Glucose Trends"),
            box(plotlyOutput("bar_chart"), title = "Average Glucose by Outcome")
          ),
          fluidRow(
            box(plotlyOutput("donut_chart"), title = "Outcome Distribution"),
            box(plotlyOutput("pie_chart"), title = "Outcome Distribution")
          )
        )
      )
    )
  )
}

# Define UI
ui <- fluidPage(
  useShinyjs(),
  useWaiter(),
  uiOutput("page_content")
)

# Server logic
server <- function(input, output, session) {
  # Reactive value to track current page
  current_page <- reactiveVal("login")
  
  # Loading screen
  w <- Waiter$new(html = spin_flower(), color = "rgba(0,0,0,0.8)")
  
  # Render current page
  output$page_content <- renderUI({
    switch(current_page(),
           "login" = loginUI(),
           "signup" = signupUI(),
           "main" = mainUI())
  })
  
  # Login handler
  observeEvent(input$login, {
    w$show()
    Sys.sleep(1)  # Simulate loading
    if (validate_login(input$username, input$password)) {
      current_page("main")
    } else {
      showModal(modalDialog(
        title = "Error",
        "Invalid credentials. Please try again.",
        easyClose = TRUE
      ))
    }
    w$hide()
  })
  
  # Signup handler
  observeEvent(input$signup, {
    w$show()
    Sys.sleep(1)  # Simulate loading
    if (input$signup_username == "" || input$signup_email == "" || 
        input$signup_mobile == "" || input$signup_password == "" || 
        input$signup_confirm_password == "") {
      showModal(modalDialog(
        title = "Error",
        "All fields are required.",
        easyClose = TRUE
      ))
    } else if (input$signup_password != input$signup_confirm_password) {
      showModal(modalDialog(
        title = "Error",
        "Passwords do not match.",
        easyClose = TRUE
      ))
    } else if (!grepl("^[^@]+@[^@]+\\.[a-z]{2,}$", input$signup_email)) {
      showModal(modalDialog(
        title = "Error",
        "Invalid email format.",
        easyClose = TRUE
      ))
    } else if (!grepl("^\\d{10}$", input$signup_mobile)) {
      showModal(modalDialog(
        title = "Error",
        "Mobile number must be 10 digits.",
        easyClose = TRUE
      ))
    } else if (email_exists(input$signup_email)) {
      showModal(modalDialog(
        title = "Error",
        "Email already registered. Please use a different email.",
        easyClose = TRUE
      ))
    } else {
      save_user_data(input$signup_username, input$signup_email, 
                     input$signup_mobile, input$signup_password)
      showModal(modalDialog(
        title = "Success",
        "Registration successful! Please login.",
        easyClose = TRUE
      ))
      current_page("login")
    }
    w$hide()
  })
  
  # Navigation handlers
  observeEvent(input$goto_signup, {
    w$show()
    Sys.sleep(0.5)
    current_page("signup")
    w$hide()
  })
  
  observeEvent(input$goto_login, {
    w$show()
    Sys.sleep(0.5)
    current_page("login")
    w$hide()
  })
  
  # Logout handler
  observeEvent(input$logout, {
    w$show()
    Sys.sleep(0.5)
    current_page("login")
    w$hide()
  })
  
  # Prediction handler
  observeEvent(input$predict, {
    w$show()
    Sys.sleep(1)
    user_input <- data.frame(
      Pregnancies = input$Pregnancies,
      Glucose = input$Glucose,
      BloodPressure = input$BloodPressure,
      SkinThickness = input$SkinThickness,
      Insulin = input$Insulin,
      BMI = input$BMI,
      DiabetesPedigreeFunction = input$DiabetesPedigreeFunction,
      Age = input$Age
    )
    prediction <- predict(model, newdata = user_input)
    result <- ifelse(prediction == 1, "High Risk", "Low Risk")
    output$prediction_result <- renderText({
      paste("Your Diabetes Risk is:", result)
    })
    w$hide()
  })
  
  # Visualization outputs with loading states
  output$visualization_plot <- renderPlotly({
    w$show()
    plot <- ggplot(diabetes_data, aes_string(x = input$x_var, y = input$y_var, color = "Outcome")) +
      geom_point() +
      theme_minimal()
    w$hide()
    ggplotly(plot)
  })
  
  output$line_chart <- renderPlotly({
    ggplot(diabetes_data, aes(x = Age, y = Glucose, color = factor(Outcome))) +
      geom_line() +
      theme_minimal()
  })
  
  output$bar_chart <- renderPlotly({
    avg_glucose <- aggregate(Glucose ~ Outcome, diabetes_data, mean)
    ggplot(avg_glucose, aes(x = factor(Outcome), y = Glucose, fill = factor(Outcome))) +
      geom_bar(stat = "identity") +
      theme_minimal()
  })
  
  output$donut_chart <- renderPlotly({
    outcome_count <- table(diabetes_data$Outcome)
    plot_ly(labels = names(outcome_count), values = outcome_count, type = 'pie', hole = 0.4)
  })
  
  output$pie_chart <- renderPlotly({
    outcome_count <- table(diabetes_data$Outcome)
    plot_ly(labels = names(outcome_count), values = outcome_count, type = 'pie')
  })
}

# Run the application

shinyApp(ui = ui, server = server)


