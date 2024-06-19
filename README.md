

# R Shiny Apps
# The purpose of this shiny app is to dynamically display a histogram of our 
# diamonds dataset where you can select the variable of interest and bin width. 

# This application is created using {shiny}, {shinydashboard}, 
# {shinydashboardPlus} and {ggplot2}.  

# You can run the application by clicking the 'Run App' button above.

library(shiny)
library(shinydashboard)
library(shinydashboardPlus)
library(ggplot2)
data(diamonds, package = 'ggplot2')


ui <- dashboardPage(
  header=dashboardHeader(title = "Diamonds Dashboard"),
  
  footer = dashboardFooter(left = "Nithi Salian", right = "Last Updated: April 15th, 2024"),
  
  sidebar=dashboardSidebar(
    sidebarMenu(
      menuItem(text = "Home" , tabName = "HomeTab" ,
               icon = icon('house')
      ),
      menuItem(text = "Graphs!" , tabName = "GraphTab" ,
               icon=icon("chart-bar")
      )
    )
  ),
  
  
  body=dashboardBody(
    tabItems(
      tabItem(tabName = "HomeTab" ,
              h1("Welcome to the Diamonds Dashboard"),
              p("Create histograms displaying distribution of selected variables and binwidths")
      ),
      tabItem(tabName="GraphTab",
              h1("Graphs!"),
              selectInput(inputId="Var2Plot",
                          label="Choose a Variable",
                          choices = c("price", "depth", "table", "carat") ,
                          selected = "price"),
              sliderInput(inputId = "Bins",
                          label= "Select your bin width",
                          min= 1, max= 50,
                          value = 30),
              plotOutput(outputId='HistPlot')
      )
    )
  )
)

server <- function(input, output){
  
  output$HistPlot <- renderPlot({
    ggplot(data = diamonds, aes_string(x=input$Var2Plot))+
      geom_histogram(bins=input$Bins)
  })
}

# Run the application 
app <- shinyApp(ui = ui, server = server)
runApp(app)
