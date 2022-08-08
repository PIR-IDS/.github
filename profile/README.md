# [Research Paper »](https://github.com/PIR-IDS/research-paper)

<!-- ABOUT THE PROJECT -->
## About The Project

Research project on the analysis of the evolution of information systems, on the critical analysis of the ability of existing attack detection solutions to support this evolution and on a proposal for an IDS (Intrusion Detection System) design method adapted to these evolutions.


<!-- USAGE -->
## Usage

This project contains 4 main repositories, each of them providing an important feature.
- The IDS logic, the detection system, is located in the 2 mobile applications, the [IDS Android App](https://github.com/PIR-IDS/IDS-Android-App) and the [IDS iOS App](https://github.com/PIR-IDS/IDS-iOS-App). These two applications are alike and are the control center of the data we will gather. We can add as many compatible Services and Devices (Probes) as we want, we just have to follow the [Android Guide](https://github.com/PIR-IDS/IDS-Android-App#contribute) and the [iOS Guide](https://github.com/PIR-IDS/IDS-iOS-App#contribute). Each Device will be a BLE Probe that will collect data and sometimes process it before sending it to the Apps.
- The third main repository is the only compatible Device yet, the [Wallet Card](https://github.com/PIR-IDS/wallet-card). It will use a ML model to produce a binary for a specified board.
- The last main repository is the training algorithm used to produce our ML model for the Wallet Card, the [Wallet Model](https://github.com/PIR-IDS/wallet-model). It will output a C++ code that will be compiled by the Wallet Card toolchain. It uses input data previously gathered.

This input data is obtained by using two other repositories, the [Wallet Data Collector](https://github.com/PIR-IDS/wallet-data-collector), which runs on the same board as the Wallet Card's and the [BLE Reader](https://github.com/PIR-IDS/ble-reader) which reads the data sent using a Bluetooth compatible computer observing the Wallet Data Collector. See the [Documentation of the Wallet Data Collector](https://github.com/PIR-IDS/wallet-data-collector#documentation) to learn more about the data we collect.

The other repositories are used as utilities and tools to do statistics about the project and to help us to achieve a better understanding on how the things work.

You can check the Release sections of each repository to get all the binaries compiled with our dataset, or if you want to use your own data, you can follow the detailled steps below.

<details>
    <summary>How to use your own dataset?</summary>

> **Note**
> All these steps have already been applied with the data we collected. This data is considered as a build artefact by the repository which produces it and as a resource by the repository which depends on it. Following this assumption, when the data is a resource, it is commited and versioned. If you want to use your own dataset, you can replace these resources by recreating the data by yourself.

1. For the Wallet Card, as it is the only compatible device yet, assuming that all the repositories prerequisites and installation steps have been satisfied and the board is plugged, we collect the data like this:

   ```sh
   cd wallet-data-collector
   pio run -e release -t upload
   cd ble-reader
   pipenv run read <address> <filename> # with <address> the device address and <filename> the file where you want the output to go in the out directory
   ```

2. When enough data is collected, we can use it to create a model. Place the content of the `ble-reader/out` directory in the `wallet-model/train` subdirectories, in `wallet` when it is a positive dataset or `negative` when it is a negative dataset, making sure the names of the data files match the pattern used by the Python scripts, then run:

   ```sh
   cd wallet-model
   pipenv run prepare && pipenv run split && pipenv run train && pipenv run generate
   ```

3. When the training is finished, copy the model file `wallet-model/output/model.cc` in the `wallet-card/res` directory and rename it `wallet_model_data.cpp`.

4. Also replace the array declaration with this one:
   ```cpp
   // Keep model aligned to 8 bytes to guarantee aligned 64-bit accesses.
   alignas(8) const unsigned char wallet_model_data[]
   ```

5. Then you can build your own binary for the board. You can run it with:

   ```sh
   cd wallet-card
   pio run -e release -t upload
   ```

</details>


<!-- CREDITS -->
## Credits

Romain Monier [ [GitHub](https://github.com/rmonier) ] – Co-developer
 | 
Noé Chauveau [ [GitHub](https://github.com/Noecv) ] – Co-developer
 | 
Amélie Muller [ [GitHub](https://github.com/AmelieMuller) ] – Co-developer
 | 
Quentin Douarre [ [GitHub](https://github.com/Quintus618) ] – Co-developer
 | 
Morgan Pelloux [ [GitHub](https://github.com/MonsieurSinge) ] – Co-developer
 | 
David Violes [ [GitHub](https://github.com/ViolesD) ] – Co-developer
 | 
Malik Sedira [ [GitHub](https://github.com/sediramalik) ] – Co-developer
 | 
Pierre Favary [ [GitHub](https://github.com/pdf-0) ] – Co-developer


<!-- CONTACT -->
## Contact

Organization Link : [https://github.com/PIR-IDS](https://github.com/PIR-IDS)

