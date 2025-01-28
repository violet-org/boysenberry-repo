## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/violet-org/boysenberry-repo/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/violet-org/boysenberry-repo/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
Code panel - press c to focus line 1.
// Copyright 2013 The Chromium Authors
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

#include "apps/app_lifetime_monitor_factory.h"

#include "apps/app_lifetime_monitor.h"
#include "components/keyed_service/content/browser_context_dependency_manager.h"
#include "content/public/browser/browser_context.h"
#include "extensions/browser/app_window/app_window_registry.h"
#include "extensions/browser/extension_host_registry.h"
#include "extensions/browser/extensions_browser_client.h"

namespace apps {

// static
AppLifetimeMonitor* AppLifetimeMonitorFactory::GetForBrowserContext(
    content::BrowserContext* context) {
  return static_cast<AppLifetimeMonitor*>(
      GetInstance()->GetServiceForBrowserContext(context, false));
}

AppLifetimeMonitorFactory* AppLifetimeMonitorFactory::GetInstance() {
  return base::Singleton<AppLifetimeMonitorFactory>::get();
}

AppLifetimeMonitorFactory::AppLifetimeMonitorFactory()
    : BrowserContextKeyedServiceFactory(
        "AppLifetimeMonitor",
        BrowserContextDependencyManager::GetInstance()) {
  DependsOn(extensions::AppWindowRegistry::Factory::GetInstance());
  DependsOn(extensions::ExtensionHostRegistry::GetFactory());
}

AppLifetimeMonitorFactory::~AppLifetimeMonitorFactory() = default;

KeyedService* AppLifetimeMonitorFactory::BuildServiceInstanceFor(
    content::BrowserContext* context) const {
  return new AppLifetimeMonitor(context);
}

bool AppLifetimeMonitorFactory::ServiceIsCreatedWithBrowserContext() const {
  return true;
}

content::BrowserContext* AppLifetimeMonitorFactory::GetBrowserContextToUse(
    content::BrowserContext* context) const {
  return extensions::ExtensionsBrowserClient::Get()->
      GetOriginalContext(context);
}

}  // namespace apps
