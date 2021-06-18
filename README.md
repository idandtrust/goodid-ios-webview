# demoprovider-ios

## How to use WKWebView with GoodID

### 1. Copy the following to the Info.plist file if necessary:

```
<key>NSAppTransportSecurity</key>
<dict>
	<key>NSAllowsArbitraryLoads</key>
	<true/>
	<key>NSAllowsArbitraryLoadsInWebContent</key>
	<true/>
</dict>
```

### 2. Use the following WKNavigationDelegate method

```objective-c
- (void)webView:(WKWebView *)webView decidePolicyForNavigationAction:(nonnull WKNavigationAction *)navigationAction decisionHandler:(nonnull void (^)(WKNavigationActionPolicy))decisionHandler {
    if ([navigationAction.request.URL.absoluteString hasPrefix:@"goodidapp://"] ||
        [navigationAction.request.URL.absoluteString hasPrefix:@"itms-appss://"]) {
        [[UIApplication sharedApplication] openURL:navigationAction.request.URL
                                           options:@{UIApplicationOpenURLOptionUniversalLinksOnly: @NO}
                                 completionHandler:^(BOOL success) {
            decisionHandler(success ? WKNavigationActionPolicyCancel : WKNavigationActionPolicyAllow);
        }];
    } else {
        decisionHandler(WKNavigationActionPolicyAllow);
    }
}
```
