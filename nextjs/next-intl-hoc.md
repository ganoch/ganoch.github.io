[next-intl](https://next-intl-docs.vercel.app/docs/getting-started) ашиглахад default-оор бүх орчуулгаа
client талруу илгээдэггүй, тиймээс орчуулга client component дээр шууд ажиллахгүй. Санал болгосон олон 
шийдлүүдийн нэг болох Provider-аар client component-оо wrap лах. Тус тусад нь байнга wrapлаад байхын оронд
дараах HOC component-ыг хэрэглэсэн нь арай амар

```tsx
const pick = require("lodash/pick");
import { NextIntlClientProvider, useMessages } from "next-intl";

// CLient component wrapper for translations
const WithTranslationsProvider = (
  WrappedComponent: any,
  translations: string[]
) => {
  const WithTranslationsProviderHOC = (props: any) => {
    // eslint-disable-next-line react-hooks/rules-of-hooks
    const messages = useMessages();
    return (
      <NextIntlClientProvider
        messages={
          // … and provide the relevant messages
          pick(messages, translations)
        }
      >
        <WrappedComponent {...props} />
      </NextIntlClientProvider>
    );
  };
  return WithTranslationsProviderHOC;
};

export default WithTranslationsProvider;
```

## usage
```tsx
"use client";
import { useTranslations } from "next-intl";

export default function Node(props) {
  const t = useTranslations("market");
  return (
      <div className="basis-1/3 flex-col pr-5">
        <p className="flex justify-between items-center">
          <span className="text-xs">{t("available")}</span>
        </p>
      </div>
  );
}
```

```tsx
import Node from "./node";

export default function Page() {
  const WrappedNode = WithTranslationsProvider(Node, ["market"]);

  return <WrappedNode />
}
```
