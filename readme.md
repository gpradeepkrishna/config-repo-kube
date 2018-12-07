# batch-service
This application uses `spring-batch` to ingest records and generate reports from them. Goal is to perform the following
 * Report errors while parsing input data
 * Generate output that lists all `workflows` and `workflow instances`.
 * Generate output that lists all `workflows` of `workflow instances` that are currently in `RUNNING` state.
 * Generate output that lists all `contractors` assigned to `workflow instances` that are currently in `RUNNING` state.

Note: For the sake of simplicity, I've used an in-memory database, it can be linked with a hosted database with relative ease.

### Assumptions
 * No restrictions on framework
 * Expect large quantities of data
 * Output format of report are not important
 * Format of input files must be as described below

## Input file record format

Typical file format has been assumed to follow format laid below.

```
Header    : WORKFLOWS
Start     : start
Attribute :   id: 1
Attribute :   name: Purchase Request Approval Sub-Workflow
Attribute :   author: john.doe@company.local
Attribute :   version: 14
End       : end
```

> `Note`: `Attribute` line format accepts either 1 `TAB` character (or) 2 `SPACE` characters before the key can start. To simplify it can be either `\tid: 1` or `  id: 1`.

## How to build?
Simply use the following command
```
./gradlew bootRun
```
> `Note`: It is important to run this in the base directory as the input files are stored in `data` directory.

### Where can I find generated reports?
```
base-directory
    |-- data [location for input files]
    |-- error [location of reports generated while parsing input data]
    `-- output [location of reports generated]
```

`Note`: Above mentioned structure is the default but can be customized. These are available as part of configuration in `application.yml`. Default configuration is listed below.

```yml
file:
  input:
    employees: file:data/employees.data
    workflows: file:data/workflow.data
    contractors: file:data/contractors.data
    workflow-instances: file:data/workflowInstances.data
  error:
    employees: file:error/employees.data
    workflows: file:error/workflow.data
    contractors: file:error/contractors.data
    workflow-instances: file:error/workflowInstances.data
  output:
    workflows-and-instances: file:output/workflows-and-instances.out
    workflows-and-running-instances: file:output/workflows-and-running-instances.out
    contractors-assigned-to-running-instances: file:output/contractors-assigned-to-running-instances.out
```

### What reports are generated?
 * `workflow-and-instances` - lists all `workflows` and its corresponding `workflow instances`;
 * `workflow-and-running-instances` - lists all `workflows` that has `workflow instances` in `RUNNING` state.
 * `contractors-assigned-to-running-instances` - lists only the contractors assigned to `workflow instance` in `RUNNING` state.

## How to execute tests?
Use the following command to execute tests

```
./gradlew test
```
