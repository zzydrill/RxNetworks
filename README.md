# RxNetworks

[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-brightgreen.svg?style=flat&colorA=28a745&&colorB=4E4E4E)](https://github.com/yangKJ/RxNetworks)
[![Releases Compatible](https://img.shields.io/github/release/yangKJ/RxNetworks.svg?style=flat&label=Releases&colorA=28a745&&colorB=4E4E4E)](https://github.com/yangKJ/RxNetworks/releases)
[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/RxNetworks.svg?style=flat&label=CocoaPods&colorA=28a745&&colorB=4E4E4E)](https://cocoapods.org/pods/RxNetworks)
![Platform](https://img.shields.io/badge/Platforms-iOS%20%7C%20macOS%20%7C%20watchOS-4E4E4E.svg?colorA=28a745)

**[RxNetworks](https://github.com/yangKJ/RxNetworks)** is a declarative and reactive networking library for Swift. Developed for Swift 5, it aims to make use of the latest language features. The framework's ultimate goal is to enable easy networking that makes it easy to write well-maintainable code.

<font color=red>**🧚. RxSwift + Moya + HandyJSON + Plugins.👒👒👒**</font>

-------

English | [**简体中文**](README_CN.md)

This is a set of infrastructure based on `RxSwift + Moya`

## Features
At the moment, the most important features of RxNetworks can be summarized as follows:

- [x] Support reactive network requests combined with [RxSwift](https://github.com/ReactiveX/RxSwift).
- [x] Support for OOP also support POP network requests.
- [x] Support data parsing with [HandyJSON](https://github.com/alibaba/HandyJSON).
- [x] Support configuration of general request and path, general parameters, etc.
- [x] Support simple customization of various network plugins for [Moya](https://github.com/Moya/Moya).
- [x] Support for injecting default plugins.
- [x] Support 6 plugins have been packaged for you to use.

### MoyaNetwork
This module is based on the Moya encapsulated network API architecture.

- Mainly divided into 8 parts:
    - [NetworkConfig](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaNetwork/NetworkConfig.swift): Set the configuration information at the beginning of the program.
        - **addDebugging**：Whether to introduce the debug mode plugin by default.
        - **baseURL**: Root path address to base URL.
        - **baseParameters**: Default basic parameters, like: userID, token, etc.
        - **baseMethod**: Default request method type.
        - **updateBaseParametersWithValue**: Update default base parameter value.
    - [RxMoyaProvider](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaNetwork/RxMoyaProvider.swift): Add responsiveness to network requests, returning `Single` sequence.
    - [NetworkUtil](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaNetwork/NetworkUtil.swift): Network related functions
         - **defaultPlugin**: Add default plugin.
         - **transformAPIObservableJSON**: Transforms a `Observable` sequence JSON object.
         - **handyConfigurationPlugin**: Handles configuration plugins.
	- [PluginSubType](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaNetwork/PluginSubType.swift): Inherit and replace the Moya plug-in protocol to facilitate subsequent expansion.
         - **configuration**: After setting the network configuration information, this method can be used in scenarios such as throwing data directly when the local cache exists without executing subsequent network requests.
         - **lastNever**: When the last network response is returned, this method can be used in scenarios such as key failure to re-obtain the key and then automatically re-request the network.
    - [NetworkAPI](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaNetwork/NetworkAPI.swift): Add protocol attributes and encapsulate basic network requests based on TargetType.
        - **ip**: Root path address to base URL.
        - **parameters**: Request parameters.
        - **plugins**: Set network plugins.
        - **stubBehavior**: Whether to take the test data.
        - **retry**：Network request failure retries.
        - **request**: Network request method and return a Single sequence object.
    - [NetworkAPI+Ext](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaNetwork/NetworkAPI+Ext.swift): Protocol default implementation scheme.
	- [NetworkAPIOO](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaNetwork/NetworkAPIOO.swift): OOP converter, MVP to OOP, convenient for friends who are used to OC thinking
        - **cdy_ip**: Root path address to base URL.
        - **cdy_path**: Request path.
        - **cdy_parameters**: Request parameters.
        - **cdy_plugins**: Set network plugins.
        - **cdy_testJSON**: Network testing json string.
        - **cdy_testTime**: Network test data return time, the default is half a second.
        - **cdy_HTTPRequest**: Network request method and return a Single sequence object.
    - [NetworkX](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaNetwork/NetworkX.swift): extension function methods etc.
        - **toJSON**: to JSON string.
        - **toDictionary**: JSON string to dictionary.
        - **+=**: Dictionary concatenation.

### MoyaPlugins
This module is mainly based on moya package network related plugins

- At present, 6 plugins have been packaged for you to use:
    - [Cache](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaPlugins/Cache/NetworkCachePlugin.swift): Network Data Cache Plugin
    - [Loading](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaPlugins/Loading/NetworkLoadingPlugin.swift): Load animation plugin
    - [Indicator](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaPlugins/Indicator/NetworkIndicatorPlugin.swift): Indicator plugin
    - [Warning](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaPlugins/Warning/NetworkWarningPlugin.swift): Network failure prompt plugin
    - [Debugging](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaPlugins/Debugging/NetworkDebuggingPlugin.swift): Network printing, built in plugin
    - [GZip](https://github.com/yangKJ/RxNetworks/blob/master/Sources/MoyaPlugins/GZip/NetworkGZipPlugin.swift): Network data unzip plugin

🏠 - Simple to use, implement the protocol method in the API protocol, and then add the plugin to it:

```
var plugins: APIPlugins {
    let cache = NetworkCachePlugin(cacheType: .networkElseCache)
    let loading = NetworkLoadingPlugin.init(delay: 0.5)
    let warning = NetworkWarningPlugin.init()
    warning.changeHud = { (hud) in
        hud.detailsLabel.textColor = UIColor.yellow
    }
    return [loading, cache, warning]
}
```

### HandyJSON
This module is based on `HandyJSON` package network data parsing

- Roughly divided into the following 3 parts:
    - [HandyDataModel](https://github.com/yangKJ/RxNetworks/blob/master/Sources/HandyJSON/HandyDataModel.swift): Network outer data model
    - [HandyJSONError](https://github.com/yangKJ/RxNetworks/blob/master/Sources/HandyJSON/HandyJSONError.swift): Parse error related
    - [RxHandyJSON](https://github.com/yangKJ/RxNetworks/blob/master/Sources/HandyJSON/RxHandyJSON.swift): HandyJSON data parsing, currently provides two parsing solutions
        - **Option 1**: Combine `HandyDataModel` model to parse out data.
        - **Option 2**: Parse the data of the specified key according to `keyPath`, the precondition is that the json data source must be in the form of a dictionary.

🎷 - Example of use in conjunction with the network part:

```
func request(_ count: Int) -> Driver<[CacheModel]> {
    CacheAPI.cache(count).request()
        .asObservable()
        .mapHandyJSON(HandyDataModel<[CacheModel]>.self)
        .compactMap { $0.data }
        .observe(on: MainScheduler.instance)
        .delay(.seconds(1), scheduler: MainScheduler.instance)
        .asDriver(onErrorJustReturn: [])
}
```

### Usage
Provide some test cases for reference.🚁

- OO Example 1:

```
class OOViewModel: NSObject {
    struct Input {
        let retry: Int
    }
    struct Output {
        let items: Observable<String>
    }
    
    func transform(input: Input) -> Output {
        return Output(items: input.request())
    }
}

extension OOViewModel.Input {
    func request() -> Observable<String> {
        var api = NetworkAPIOO.init()
        api.cdy_ip = NetworkConfig.baseURL
        api.cdy_path = "/ip"
        api.cdy_method = APIMethod.get
        api.cdy_plugins = [NetworkLoadingPlugin()]
        api.cdy_retry = self.retry
        
        return api.cdy_HTTPRequest()
            .asObservable()
            .compactMap{ (($0 as! NSDictionary)["origin"] as? String) }
            .catchAndReturn("")
            .observe(on: MainScheduler.instance)
    }
}
```

- MVP Example 2:

```
enum LoadingAPI {
    case test2(String)
}

extension LoadingAPI: NetworkAPI {
    var ip: APIHost {
        return NetworkConfig.baseURL
    }
    
    var path: String {
        return "/post"
    }
    
    var parameters: APIParameters? {
        switch self {
        case .test2(let string): return ["key": string]
        }
    }
}


class LoadingViewModel: NSObject {
    let disposeBag = DisposeBag()
    let data = PublishRelay<NSDictionary>()
    
    /// Configure the loading animation plugin
    let APIProvider: MoyaProvider<MultiTarget> = {
        let configuration = URLSessionConfiguration.default
        configuration.headers = .default
        configuration.timeoutIntervalForRequest = 30
        let session = Moya.Session(configuration: configuration, startRequestsImmediately: false)
        let loading = NetworkLoadingPlugin.init()
        return MoyaProvider<MultiTarget>(session: session, plugins: [loading])
    }()
    
    func loadData() {
        APIProvider.rx.request(api: LoadingAPI.test2("666"))
            .asObservable()
            .subscribe { [weak self] (event) in
                if let dict = event.element as? NSDictionary {
                    self?.data.accept(dict)
                }
            }.disposed(by: disposeBag)
    }
}
```

- MVVM Example 3:

```
class CacheViewModel: NSObject {
    struct Input {
        let count: Int
    }
    struct Output {
        let items: Observable<[CacheModel]>
    }
    
    func transform(input: Input) -> Output {
        let items = request(input.count).asObservable()
        
        return Output(items: items)
    }
}

extension CacheViewModel {
    
    func request(_ count: Int) -> Observable<[CacheModel]> {
        CacheAPI.cache(count).request()
            .mapHandyJSON(HandyDataModel<[CacheModel]>.self)
            .compactMap { $0.data }
            .observe(on: MainScheduler.instance) // the result is returned on the main thread
            .catchAndReturn([]) // return null on error
    }
}
```

- Chain Example 4:

```
class ChainViewModel: NSObject {
    let disposeBag = DisposeBag()
    let data = PublishRelay<NSDictionary>()
    
    func chainLoad() {
        requestIP()
            .flatMapLatest(requestData)
            .subscribe(onNext: { [weak self] data in
                self?.data.accept(data)
            }, onError: {
                print("Network Failed: \($0)")
            }).disposed(by: disposeBag)
    }
}

extension ChainViewModel {
    func requestIP() -> Observable<String> {
        return ChainAPI.test.request()
            .asObservable()
            .map { ($0 as! NSDictionary)["origin"] as! String }
            .catchAndReturn("") // Exception thrown
    }
    
    func requestData(_ ip: String) -> Observable<NSDictionary> {
        return ChainAPI.test2(ip).request()
            .asObservable()
            .map { ($0 as! NSDictionary) }
            .catchAndReturn(["data": "nil"])
    }
}
```

- Batch Example 5:

```
class BatchViewModel: NSObject {
    let disposeBag = DisposeBag()
    let data = PublishRelay<NSDictionary>()
    
    /// Configure loading animation plugin
    let APIProvider: MoyaProvider<MultiTarget> = {
        let configuration = URLSessionConfiguration.default
        configuration.headers = .default
        configuration.timeoutIntervalForRequest = 30
        let session = Moya.Session(configuration: configuration, startRequestsImmediately: false)
        let loading = NetworkLoadingPlugin.init()
        return MoyaProvider<MultiTarget>(session: session, plugins: [loading])
    }()
    
    func batchLoad() {
        Observable.zip(
            APIProvider.rx.request(api: BatchAPI.test).asObservable(),
            APIProvider.rx.request(api: BatchAPI.test2("666")).asObservable(),
            APIProvider.rx.request(api: BatchAPI.test3).asObservable()
        ).subscribe(onNext: { [weak self] in
            guard var data1 = $0 as? Dictionary<String, Any>,
                  let data2 = $1 as? Dictionary<String, Any>,
                  let data3 = $2 as? Dictionary<String, Any> else {
                      return
                  }
            data1 += data2
            data1 += data3
            self?.data.accept(data1)
        }, onError: {
            print("Network Failed: \($0)")
        }).disposed(by: disposeBag)
    }
}
```

### CocoaPods

[CocoaPods](https://cocoapods.org) is a dependency manager. For usage and installation instructions, visit their website. To integrate using CocoaPods, specify it in your Podfile:

```
pod 'RxNetworks'
```

You should define your minimum deployment target explicitly, like: 

```
platform :ios, '10.0'
```

If you only want import loading animation plugin:

```
pod 'RxNetworks/MoyaPlugins/Loading'
```

### Remarks

> The general process is almost like this, the Demo is also written in great detail, you can check it out for yourself.🎷
>
> [**RxNetworksDemo**](https://github.com/yangKJ/RxNetworks)
>
> Tip: If you find it helpful, please help me with a star. If you have any questions or needs, you can also issue.
>
> Thanks.🎇

### About the author
- 🎷 **E-mail address: [yangkj310@gmail.com](yangkj310@gmail.com) 🎷**
- 🎸 **GitHub address: [yangKJ](https://github.com/yangKJ) 🎸**

-----

### License
RxNetworks is available under the [MIT](LICENSE) license. See the [LICENSE](LICENSE) file for more info.

-----
