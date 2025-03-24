# Sercos

# Sercos FSM
The `FB_Sercos_FSM` provides a simulation of the [Sercos III Operating State Diagram](https://product-help.schneider-electric.com/Machine%20Expert/V1.2/en/ATV340SUG/ATV340SUG/STI_Software_Setup/STI_Software_Setup-19.htm#XREF_D_SE_0083217_1).

# Sercos_VS
The `FB_Sercos_VC` provides a simulation of the Sercos Velocity Control drive.

## Interface
* MDT: S-0-0134 Master control word
* MDT: S-0-0036 Velocity command value
* AT: S-0-0135 Drive Status word
* AT: S-0-0051 Position feedback 1 value

## Configuration
Configuration is represendet in `ST_Sercos_VC_Config`:

```js
bForceMode                  : BOOL; //Ignore process data from fieldbus.	
bSwapProcessData            : BOOL; //Reverse process data byte order from/to fieldbus.	
bIsLinkOutputVelocity	    : BOOL; //If TRUE, link output is velocity value
fReferenceVelocity          : REAL := 1000.0;
fNormalizationFactor	    : REAL := 80000;
```

* **fReferenceVelocity:** this parameter defines the reference value used to convert the CommandVelocity from DINT to REAL. It corresponds to the velocity scaling setting in the drive's parameter table and can typically be found in the NS-Task configuration of the drive. ![Sercos_Config_ReferenceVelocity.png](Images%2FSercos_Config_ReferenceVelocity.png)
* **fNormalizationFactor:** This value defines the upper limit used for normalization. It represents the raw drive output corresponding to 100% in the VelocityCommand channel. In other words, when the normalized velocity reaches 1.0 (or 100%), the output equals this factor.
![Sercos_Config_NormalizationFactor.png](Images%2FSercos_Config_NormalizationFactor.png)
* **Encoder.stConfig:** Ensure that the encoder configuration parameters are correctly set. These parameters can be found in the Encoder Parameter Table within the NS-Task component.
```js
bIsIntegrator               : BOOL := FALSE; //Integrate input value
fNumerator                  : REAL := 0.0001; //[unit/INC]
fDenominator                : REAL := 1.0; // Scaling
fModulo                     : REAL := 0; // Modulo [unit]; If 0, will be ignored
fGearFactor                 : REAL := 1.0;
bInvertEncoderDirection     : BOOL;
```
![Sercos_Config_Encoder.png](Images%2FSercos_Config_Encoder.png)

