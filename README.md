To create a REST API for the CRUD operations on the `Elevator` and `ElevatorInfo` entities using Spring Boot, you can follow the steps outlined in the [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/) guide with modifications specific to your entities.

I'll provide a simplified example for the `Elevator` entity, and you can extend it to the `ElevatorInfo` entity and other CRUD operations:

### Step 1: Create the `ElevatorController` class

Create a controller class for the `Elevator` entity. This class will handle HTTP requests for CRUD operations.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/elevators")
public class ElevatorController {

    @Autowired
    private ElevatorService elevatorService;

    // Retrieve all elevators
    @GetMapping
    public List<Elevator> getAllElevators() {
        return elevatorService.getAllElevators();
    }

    // Retrieve a specific elevator by ID
    @GetMapping("/{id}")
    public Elevator getElevatorById(@PathVariable Long id) {
        return elevatorService.getElevatorById(id);
    }

    // Create a new elevator
    @PostMapping
    public Elevator createElevator(@RequestBody Elevator elevator) {
        return elevatorService.createElevator(elevator);
    }

    // Update an existing elevator
    @PutMapping("/{id}")
    public Elevator updateElevator(@PathVariable Long id, @RequestBody Elevator elevator) {
        return elevatorService.updateElevator(id, elevator);
    }

    // Delete an elevator by ID
    @DeleteMapping("/{id}")
    public void deleteElevator(@PathVariable Long id) {
        elevatorService.deleteElevator(id);
    }
}
```

### Step 2: Create the `ElevatorService` class

Create a service class that handles the business logic for the `Elevator` entity.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class ElevatorService {

    @Autowired
    private ElevatorRepository elevatorRepository;

    // Retrieve all elevators
    public List<Elevator> getAllElevators() {
        return elevatorRepository.findAll();
    }

    // Retrieve a specific elevator by ID
    public Elevator getElevatorById(Long id) {
        return elevatorRepository.findById(id).orElse(null);
    }

    // Create a new elevator
    public Elevator createElevator(Elevator elevator) {
        return elevatorRepository.save(elevator);
    }

    // Update an existing elevator
    public Elevator updateElevator(Long id, Elevator updatedElevator) {
        Elevator existingElevator = elevatorRepository.findById(id).orElse(null);
        if (existingElevator != null) {
            // Update fields as needed
            existingElevator.setCurrentFloor(updatedElevator.getCurrentFloor());
            // ... other fields

            return elevatorRepository.save(existingElevator);
        }
        return null; // Handle not found case
    }

    // Delete an elevator by ID
    public void deleteElevator(Long id) {
        elevatorRepository.deleteById(id);
    }
}
```

### Step 3: Create the `ElevatorRepository` interface

Create a repository interface for the `Elevator` entity.

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ElevatorRepository extends JpaRepository<Elevator, Long> {
}
```

### Step 4: Update `ElevatorApplication`

Make sure your main application class (`ElevatorApplication`) is in the correct package and annotated with `@SpringBootApplication`.

### Step 5: Run the Application

Run your Spring Boot application, and the REST API for CRUD operations on `Elevator` should be available at `http://localhost:8080/api/elevators`.

You can follow a similar pattern for the `ElevatorInfo` entity by creating a `ElevatorInfoController`, `ElevatorInfoService`, and `ElevatorInfoRepository`. Ensure that you modify the code according to your specific requirements.

Feel free to ask if you have any questions or need further clarification on any part of the implementation.
