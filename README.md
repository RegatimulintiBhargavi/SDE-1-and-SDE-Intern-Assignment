# SDE-1-and-SDE-Intern-Assignment
# Log Ingester

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;

@SpringBootApplication
public class LogIngestorApplication {

    public static void main(String[] args) {
        SpringApplication.run(LogIngestorApplication.class, args);
    }
}

@RestController
class LogController {

    private final List<Log> logs = new ArrayList<>();

    @PostMapping("/ingest")
    public String ingestLog(@RequestBody Log newLog) {
        logs.add(newLog);
        return "Log ingested successfully!";
    }
}

class Log {
    // Log fields here
}
# Query Interface
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;
import java.util.stream.Collectors;

@SpringBootApplication
public class QueryInterfaceApplication {

    public static void main(String[] args) {
        SpringApplication.run(QueryInterfaceApplication.class, args);
    }
}

@RestController
class QueryController {

    private final List<Log> logs = new ArrayList<>();

    @GetMapping("/search")
    public List<Log> searchLogs(@RequestParam(required = false) String level,
                                @RequestParam(required = false) String message,
                                @RequestParam(required = false) String resourceId,
                               
    ) {
        return filterLogs(level, message, resourceId);
    }

    private List<Log> filterLogs(String level, String message, String resourceId) {
        List<Log> filteredLogs = logs;
        if (level != null) {
            filteredLogs = filteredLogs.stream().filter(log -> level.equals(log.getLevel())).collect(Collectors.toList());
        }
       
        return filteredLogs;
    }
}

class Log {
   
}
