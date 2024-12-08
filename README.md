# Event Modeling for PlantUML

Provides a library for [Event Modeling](https://eventmodeling.org) with
[PlantUML](https://plantuml.com). The library contains the following:

Elements:

- `UI(alias, "label")`
- `Processor(alias, "label")`
- `Command(alias, "label")`
- `Query(alias, "label")`
- `Event(alias, "label")`

Containers:

- `Role(alias, "label")`
- `Aggregate(alias, "label")`

Relationships:

- `Rel_D(from, to)` / `Rel_Up(from, to)`
- `Rel_U(from, to)` / `Rel_Down(from, to)`
- `Lay_R(from, to)` / `Lay_Right(from, to)`

## Installation

Add
`!include https://raw.githubusercontent.com/falkoschumann/Event-Modeling-PlantUML/main/Event-Modeling.puml`
to your PlantUML file.

## Usage

See https://eventmodeling.org/posts/event-modeling-cheatsheet/

### Command Pattern

```plantuml
@startuml
!include ./Event-Modeling.puml

Role(role1, "Role 1") {
  UI(ui, "UI")
}

Role(role2, "Role 2") {
  UI(postAppointment, "POST /appointment")
}

Command(addAppointment, "Add appointment")

Aggregate(appointment, "Appointment") {
  Event(appointmentAdded, "Appointment added")
}

Rel_Down(ui, addAppointment)
Rel_Down(postAppointment, addAppointment)
Rel_Down(addAppointment, appointmentAdded)
@enduml
```

Source:

```
@startuml
!include ./Event-Modeling.puml

Role(role1, "Role 1") {
  UI(ui, "UI")
}

Role(role2, "Role 2") {
  UI(postAppointment, "POST /appointment")
}

Command(addAppointment, "Add appointment")

Aggregate(appointment, "Appointment") {
  Event(appointmentAdded, "Appointment added")
}

Rel_Down(ui, addAppointment)
Rel_Down(postAppointment, addAppointment)
Rel_Down(addAppointment, appointmentAdded)
@enduml
```

### View Pattern

```plantuml
@startuml
!include ./Event-Modeling.puml

Role(role1, "Role 1") {
  UI(ui, "UI")
}

Role(role2, "Role 2") {
  UI(getAppointment, "GET /appointment")
}

Query(calender, "Calender")

Aggregate(appointment, "Appointment") {
  Event(appointmentAdded, "Appointment added")
}

Rel_Up(appointmentAdded, calender)
Rel_Up(calender, ui)
Rel_Up(calender, getAppointment)
@enduml
```

Source:

```
@startuml
!include ./Event-Modeling.puml

Role(role1, "Role 1") {
  UI(ui, "UI")
}

Role(role2, "Role 2") {
  UI(getAppointment, "GET /appointment")
}

Query(calender, "Calender")

Aggregate(appointment, "Appointment") {
  Event(appointmentAdded, "Appointment added")
}

Rel_Up(appointmentAdded, calender)
Rel_Up(calender, ui)
Rel_Up(calender, getAppointment)
@enduml
```

### Automation Pattern

```plantuml
@startuml
!include ./Event-Modeling.puml

Processor(weatherProcessor, "Weather Processor")

Query(appointmentsWithoutWeatherForecast, "Appointments without weather forecast")
Command(addWeatherForecast, "Add weather forecast")

Lay_Right(appointmentsWithoutWeatherForecast, addWeatherForecast)

Aggregate(appointment, "Appointment") {
  Event(appointmentAdded, "Appointment added")
}

Aggregate(weather, "Weather") {
  Event(weatherPredictedForAppointment, "Weather predicted for appointment")
}

Rel_Up(appointmentAdded, appointmentsWithoutWeatherForecast)
Rel_Up(appointmentsWithoutWeatherForecast, weatherProcessor)
Rel_Down(weatherProcessor, addWeatherForecast)
Rel_Down(addWeatherForecast, weatherPredictedForAppointment)
@enduml
```

Source:

```
@startuml
!include ./Event-Modeling.puml

Processor(weatherProcessor, "Weather Processor")

Query(appointmentsWithoutWeatherForecast, "Appointments without weather forecast")
Command(addWeatherForecast, "Add weather forecast")

Lay_Right(appointmentsWithoutWeatherForecast, addWeatherForecast)

Aggregate(appointment, "Appointment") {
  Event(appointmentAdded, "Appointment added")
}

Aggregate(weather, "Weather") {
  Event(weatherPredictedForAppointment, "Weather predicted for appointment")
}

Rel_Up(appointmentAdded, appointmentsWithoutWeatherForecast)
Rel_Up(appointmentsWithoutWeatherForecast, weatherProcessor)
Rel_Down(weatherProcessor, addWeatherForecast)
Rel_Down(addWeatherForecast, weatherPredictedForAppointment)
@enduml
```

### Translation Pattern

```plantuml
@startuml
!include ./Event-Modeling.puml

Processor(translatorProcessor, "Translator Processor")

Query(changedPredictions, "Changed predictions")
Command(translateChangedWeather, "Translate changed weather")

Lay_Right(changedPredictions, translateChangedWeather)

Aggregate(weather, "Weather") {
  Event(updatedWeatherPrediction, "Updated weather prediction")
}

Aggregate(someExternalWeatherApi, "Some external weather API") {
  Event(weatherForecastChanged, "Add weather forecast")
}

Rel_Up(weatherForecastChanged, changedPredictions)
Rel_Up(changedPredictions, translatorProcessor)
Rel_Down(translatorProcessor, translateChangedWeather)
Rel_Down(translateChangedWeather, updatedWeatherPrediction)
@enduml
```

Source:

```
@startuml
!include ./Event-Modeling.puml

Processor(translatorProcessor, "Translator Processor")

Query(changedPredictions, "Changed predictions")
Command(translateChangedWeather, "Translate changed weather")

Lay_Right(changedPredictions, translateChangedWeather)

Aggregate(weather, "Weather") {
  Event(updatedWeatherPrediction, "Updated weather prediction")
}

Aggregate(someExternalWeatherApi, "Some external weather API") {
  Event(weatherForecastChanged, "Add weather forecast")
}

Rel_Up(weatherForecastChanged, changedPredictions)
Rel_Up(changedPredictions, translatorProcessor)
Rel_Down(translatorProcessor, translateChangedWeather)
Rel_Down(translateChangedWeather, updatedWeatherPrediction)
@enduml
```

## Credits

This implementation based on https://github.com/plantuml/plantuml-stdlib.
