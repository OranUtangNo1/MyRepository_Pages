```mermaid
classDiagram
    %% クラス定義
    class WpfDigitalDistortion
    class DigitalDistortion_VM
    class DigitalDistortion_Orchestra
    class WaveformDataPorter
    class AveragedWaveData
    class OperationResult
    class IoParamSet

    %% ステレオタイプ付与
    <<Window>> WpfDigitalDistortion
    <<ViewModel>> DigitalDistortion_VM
    <<Orchestrator>> DigitalDistortion_Orchestra
    <<Interface>> IWaveformImporter
    <<Interface>> IWaveformExporter
    <<Data>> AveragedWaveData
    <<Data>> OperationResult

    %% 関係性：Presentation & Orchestration
    WpfDigitalDistortion ..> DigitalDistortion_VM : DataContext
    DigitalDistortion_VM --> DigitalDistortion_Orchestra : Calls methods
    DigitalDistortion_Orchestra --> OperationResult : Returns

    %% 関係性：Orchestraによる強結合 (Composition)
    DigitalDistortion_Orchestra *-- PeriodEstimator
    DigitalDistortion_Orchestra *-- ScanDelayCalculator
    DigitalDistortion_Orchestra *-- WaveformAverager
    DigitalDistortion_Orchestra *-- WaveformLinearCorrecter
    DigitalDistortion_Orchestra *-- WaveformSegmenter
    DigitalDistortion_Orchestra *-- LinearCorrectionValueCalculator
    DigitalDistortion_Orchestra *-- WaveformDataPorter

    %% 関係性：DI (Dependency Injection)
    WaveformDataPorter o-- IWaveformImporter : DI
    WaveformDataPorter o-- IWaveformExporter : DI
    WaveformDataPorter o-- IoController : DI
    
    IWaveformImporter <|.. WaveformCsvLoader : Implements
    IWaveformExporter <|.. WaveformCsvExporter : Implements

    %% 関係性：IO & Params
    IoController --> IoParamSet : Uses
    IoParamSet *-- IoParameter : Contains
    WaveformDataPorter ..> OperationResult : Returns

    %% 関係性：Data Flow
    DigitalDistortion_Orchestra ..> AveragedWaveData : Creates / Holds
    
    %% Exception (関連クラスとして集約表示)
    class Exceptions {
        CsvException
        IOControlException
        WaveformAnalysisException
    }
    WaveformDataPorter ..> Exceptions : Throws
    DigitalDistortion_Orchestra ..> Exceptions : Throws
```
