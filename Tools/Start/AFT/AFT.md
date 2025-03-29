- 根据前述扫码状况，并设定config.ini
- 其中S1-S6，分别代表不同的自动化设备，如果扫码到那个设备，则这个设备改为自动测试，手动的disable
- AMD CPU，则修改config.ini为TPM MSFT
- 如果发现S3或者manual，则修改C:\Tools\camera中的配置档案
- 显示SN二维码，同时显示一个测试模式的二维码
- 调起ASSY_TestCases.exe测试function功能

```mermaid
graph TD
    A[Start] --> B[Call IMAGEVER.bat]
    B --> C[EXITQR]
    C --> D[Write 123 to C:\EXITQR.log]
    D --> E[EXITRUNIN]
    E --> F[Change Directory to \TOOLS]
    F --> G[Kill RuninTest.exe]
    G --> H[Delete RunIn_startup Registry Key]
    H --> I{BOM.bat Exists?}
    I --No--> J[Call getbom.bat]
    J --> K[Call BOM.bat]
    I --Yes--> K
    K --> L[desktoplabel]
    L --> M[Kill desktoplabel.exe]
    M --> N[Wait 1 second]
    N --> O[Start desktoplabel.exe]
    O --> P[speak vol 100]
    P --> Q[Set System Volume to 100]
    Q --> R[WLAN Disconnect]
    R --> S[Disconnect WLAN]
    S --> T[Delete All WLAN Profiles]
    T --> U[LAN]
    U --> V[Change Directory to SetBootToLan]
    V --> W[Call SetBootToLan.cmd]
    W --> X[LID]
    X --> Y[Change Directory to \TOOLS\LIDdoNothing]
    Y --> Z[Call LIDnormal.cmd]
    Z --> AA[repair]
    AA --> AB{CRT_STATION=Repair in BOM.bat?}
    AB --Yes--> AC[Copy INI\configM.ini to config.ini]
    AB --No--> AD{Model == DN39-140H-YD?}
    AD --Yes--> AC
    AD --No--> AE[ScanQR]
    AE --> AF[Change Directory to \TOOLS\ScanQR1]
    AF --> AG[Call ScanQR.bat]
    AG --> AH[Change Directory to \TOOLS]
    AH --> AI{Result Empty?}
    AI --Yes--> AJ[ScanQRFAIL]
    AI --No--> AK[CheckManu]
    AK --> AL{Result Contains 'manual'?}
    AL --Yes--> AC
    AL --No--> AM{Mode == manual?}
    AM --Yes--> AC
    AM --No--> AN[CheckLine]
    AN --> AO[Loop through A to N]
    AO --> AP{Result Contains Line Letter?}
    AP --Yes--> AQ[Copy INI\configX.ini to config.ini]
    AP --No--> AR[Check Line Name FAIL]
    AR --> AS[Pause]
    AS --> AT[Go to ScanQR]
    AQ --> AU[S1]
    AU --> AV{Result Contains 'S1'?}
    AV --Yes--> AW[Modify config.ini for S1]
    AV --No--> AX[S2]
    AX --> AY{Result Contains 'S2'?}
    AY --Yes--> AZ[Modify config.ini for S2]
    AY --No--> BA[S3]
    BA --> BB{Result Contains 'S3'?}
    BB --Yes--> BC[Modify config.ini for S3]
    BB --No--> BD[S4]
    BD --> BE{Result Contains 'S4'?}
    BE --Yes--> BF[Modify config.ini for S4]
    BE --No--> BG[S5]
    BG --> BH{Result Contains 'S5'?}
    BH --Yes--> BI[Modify config.ini for S5]
    BH --No--> BJ[S6]
    BJ --> BK{Result Contains 'B', 'C', or 'S6'?}
    BK --Yes--> BL[IO]
    BK --No--> BM[TPM]
    BL --> BN[Modify config.ini for IO]
    BN --> BM
    BM --> BO{CPU Name Contains 'AMD'?}
    BO --Yes--> BP[Modify config.ini for TPM MSFT]
    BO --No--> BQ[TPM]
    BQ --> BR[Change Directory to \TOOLS\Camera]
    BR --> BS[Modify config.ini for Camera]
    BS --> BT{Result Contains 'S3'?}
    BT --Yes--> BU[Modify config.ini for Capture ALL]
    BT --No--> BV{Result Contains 'manual'?}
    BV --Yes--> BU
    BV --No--> BW[ShowQR]
    BW --> BX[Delete C:\EXITQR.log]
    BX --> BY[Change Directory to \TOOLS\ShowQR]
    BY --> BZ[Delete EXITQR.log]
    BZ --> CA[Start ShowQR.bat]
    CA --> CB[TEST]
    CB --> CC[Change Directory to \TOOLS]
    CC --> CD[Wait 1 second]
    CD --> CE[Start ASSY_TestCases.exe]
    CE --> CF[Wait 1 second]
    CF --> CG[Exit]

    AJ --> CH[Color 4f]
    CH --> CI[Print Scan QR FAIL]
    CI --> CJ[Pause]
    CJ --> CK[Go to Start]
