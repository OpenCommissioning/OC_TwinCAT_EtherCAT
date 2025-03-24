# Open Commissioning: TwinCAT EtherCAT Library

**OC_EtherCAT** is a library for TwinCAT designed to emulate [EtherCAT](https://www.beckhoff.com/de-de/produkte/i-o/ethercat/#text_bild_1) devices. It includes structs and functional blocks for describing the behavior of EtherCAT devices and supports connection between Open Commissioning Project and [EtherCAT Simulation](https://www.beckhoff.com/de-de/produkte/automation/twincat/texxxx-twincat-3-engineering/te1111.html).

## Setup
+ **Add the Library**: Download the `OC_EtherCAT.library` and add it to the Open Commissioning PLC project using the Library Manager in TwinCAT, just as you would with any standard TwinCAT library.

+ **Add the Mapping Config**: Download the `OC_EtherCAT.ethml` file and copy it into your TwinCAT solution. This file is used as an EtherCAT Device Mapping Template, for generating GVL variables with the [Assistant](https://github.com/OpenCommissioning/OC_Assistant).

## Workflow
### 1. EtherCAT Simulation
The first step in the workflow is to create an EtherCAT emulation device. This involves setting up the real EtherCAT project on your PLC and export its configuration to the simulation environment. 

Use Beckhoff’s [EtherCAT Simulation](https://www.beckhoff.com/de-de/produkte/automation/twincat/texxxx-twincat-3-engineering/te1111.html)  to add a new device on the simulation side. Import the exported hardware configuration file into this emulated device. After importing, you will see an exact representation of the emulated section of your EtherCAT fieldbus on the simulation side.

![EtherCAT_Emulation.PNG](Documentation%2FImages%2FEtherCAT_Emulation.PNG)

![EtherCAT_Emulation_Device.PNG](Documentation%2FImages%2FEtherCAT_Emulation_Device.PNG)

> [!NOTE]  
> Ensure that all devices have complete information about their type, object ID, and other relevant attributes. This data is crucial for the generation.

### 2. Generation
Once the EtherCAT Simulation device is set up, you can proceed the generation using the Assistant's Rebuild Project function. The Assistant scans the EtherCAT Simulation device and generates a GVL variable for each item, applying the specific mapping template defined in the solution configuration files `.ethml`.

![EtherCAT_Device_Mapping.png](Documentation%2FImages%2FEtherCAT_Device_Mapping.png)

The TwinCAT [Attribute 'TcLinkTo'](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_plc_intro/3107974923.html&id=) is used to establish the connection between variables and the emulated device. Upon starting the project, these linked variables are automatically connected, with a blue link icon visually indicating the established connection.

> [!WARNING]  
> The generation process requires a mapping template for all devices. If a template is not found for a specific device, a warning will appear in the Assistant Console, alerting you to the missing configuration.
> ![EtherCAT_Device_Config_Warning.png](Documentation%2FImages%2FEtherCAT_Device_Config_Warning.png)

#### Mapping Template
A new template can be created manually if needed. To create a template, add a new device description in the configuration file and define the variable links within the description section. Use or create a specific `STRUCT` or `FUNCTION_BLOCK` to represent the interface of a device. Leverage TwinCAT's attribute syntax to establish connections between variables.
Below is an example of Template:

```xml
<Device ProductDescription="EL1008">
    <Declaration>
        <![CDATA[{attribute 'TcLinkTo' := '
        .bIn1 := TIIB($BOXNO$)^Channel 1^Input;
        .bIn2 := TIIB($BOXNO$)^Channel 2^Input;
        .bIn3 := TIIB($BOXNO$)^Channel 3^Input;
        .bIn4 := TIIB($BOXNO$)^Channel 4^Input;
        .bIn5 := TIIB($BOXNO$)^Channel 5^Input;
        .bIn6 := TIIB($BOXNO$)^Channel 6^Input;
        .bIn7 := TIIB($BOXNO$)^Channel 7^Input;
        .bIn8 := TIIB($BOXNO$)^Channel 8^Input'}
        $NAME$ : ST_Beckhoff_8DI;]]>
    </Declaration>
</Device>
```

![EtherCAT_Mapping.drawio.png](Documentation%2FImages%2FEtherCAT_Mapping.drawio.png)

Make sure that the type name of the `ProductDescription` is correct, as it is used as the key for finding the corresponding assignment template in the configuration files.

### 3. Usage
Once the GVL is generated, it can be utilized in the Open Commissioning TwinCAT Project to establish connections with other components. Below is an example demonstrating how to couple with specific components:

![EtherCAT_Project_Mapping.png](Documentation%2FImages%2FEtherCAT_Project_Mapping.png)

### 4. Components
* [Sercos Drives](Documentation%2FSercos.md)

## Support
If you create your own module or encounter specific challenges, we encourage you to:
+ Open a discussion in the [GitHub Discussions](https://github.com/orgs/OpenCommissioning/discussions) section to share your ideas or issues.
+ Collaborate with the community to enhance the library and its functionality.

## Contributing
We welcome contributions from everyone and appreciate your effort to improve this project.
We have some basic rules and guidelines that make the contributing process easier for everyone involved.

### Submitting Pull Requests
1. For non-trivial changes, please open an issue first to discuss your proposed changes.
2. Fork the repo and create your feature branch.
3. Follow the code style conventions and guidelines throughout working on your contribution.
4. Create a pull request with a clear title and description.

> [!NOTE]
> All contributions will be licensed under the project's license

### Code Style Convention
Please follow [Beckhoff Programming Conventions](https://infosys.beckhoff.com/english.php?content=../content/1033/tc3_plc_intro/12049233675.html&id=6398798947359024199) in your code.

### Guidelines for Contributions
- **Keep changes focused:** Submit one pull request per bug fix or feature. This makes it easier to review and merge your contributions.
- **Discuss major changes:** For large or complex changes, please open an issue to discuss with maintainers before starting work.
- **Commit message format**: Use the [semantic-release](https://semantic-release.gitbook.io/semantic-release#commit-message-format) commit message format.
- **Write clear code:** Prioritize readability and maintainability.
- **Be consistent:** Follow existing coding styles and patterns in the project.
- **Include tests:** It is recommended to add or update tests to cover your changes.
- **Document your work:** Update relevant documentation, including code comments and user guides.