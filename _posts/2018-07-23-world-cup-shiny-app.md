The data for World Cup goal times caused me a bit of a headache, and I was happy to get everything sorted out so I could start exploring something new.  Nevertheless, an idea for one last visualization using this data has been stuck in my head.  

My favorite plot to come out of the original analysis was the distribution of goal times broken down by year.  However, there were so many years that I was never able to get the dimensions of the plot to look quite right.  The histograms always seemed distorted in some way.  To fix this, I decided to try an interactive Shiny app where one could select individual years to visualize.

The visualization I have in mind is really simple and shouldn't take too much coding.  And yes, that is exactly what I *incorrectly* assumed when I initially scraped the data.  Fortunately, things went better this time.

First off, I need to load the data and necessary packages.  Since this app is going to be very simple, two packages will do:
```R
library(shiny)
library(ggplot2)

# data frame is called new.goals
load("updated_goals.RData")
```
Shiny apps have two main pieces: the user interface defintions in `ui` and the `server` function.  I want my interface to have a title panel, year input selection on the side, and plot in the main panel.  I can get that with:
```R
ui <- fluidPage(

  headerPanel("When World Cup Goals Are Scored"),
  
  sidebarPanel(
    selectInput("var", label = "Select year:", unique(new.goals$year))
  ),
  
  mainPanel(
    plotOutput("hist")
  )
)
```
Then, my server function takes the user-selected year to subset the data to produce a histrogram in `ggplot2`.
```R
server <- function(input, output) {
   
  selectedYear <- reactive({
    subset(new.goals, year == input$var)
  })
  
  output$hist <- renderPlot({
    ggplot(data = selectedYear(), aes(x = time)) + geom_histogram(breaks = seq(0, 125, 5), color = 'black', fill = 'steelblue3') +
      labs(x = 'Game Time', y = 'Count')
  })
  
}
```
Finally, running `shinyApp(ui = ui, server = server)` will launch the app locally.  

Shiny is very cool and there certainly is enough going on to justify a more in-depth post at some point in the future, but for now, this app is up and running on my [shinyapps.io](https://tylerlewiscook.shinyapps.io/WorldCupGoals/) page.
