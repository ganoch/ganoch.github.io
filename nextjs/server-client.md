### server actions

React-ын mod-нд байхгүй тул

- Component зурахгүй (no tsx)
- client lib-ууд ажиллахгүй (react-cookie гэх мэт)
- hooks байхгүй
- цэвэр сэрвэр function болон class-ууд
- заавал `"use server"` болгож тодорхойлно

```ts
"use server";
// non arrow function example
export async function doSomethingServerSide(formData) {
  console.log(formData.get("username"));
  return { success: true, message: "Wow! it works" };
}

// arrow function example
export const doSomethingElseServerSide = async () => {
  return { success: true, message: "Wow! it works" };
};
```

usage:

```tsx
"use client";
export function SomeComponent() {
  return <form action={doSomethingServerSide}></form>;
}

export function SomeOtherComponent() {
  useEffect(() => {
    doSomethingElseServerSide().then((returnValue) => {
      console.log(returnValue.message);
    });
  }, []);
  return <div></div>;
}
```
### server component async, server components

React талдаа их боогдмол component тул аль болох shared-ыг ашиглахыг хичээх

- өөр дээрээ шууд data load хийнэ, useEffect хэрэглэхгүй болохоор.
- hooks ажиллахгүй
- client үйлдлүүд ажиллахгүй

```tsx
import { getServerSession } from "next-auth/next";
import { getTranslations } from "next-intl/server";

export default async function Page() {
  const data = await doSomethingElseServerSide();
  const session = await getServerSession();
  const t = await getTranslations("global");
  return <div>{t("ehlo world")}</div>;
}
```

### server component non-async буюу shared component:

- server дээрээ ч client дээр ч ажиллана renderлэгдэнэ
- hooks ажиллана (useTranslations, useMessage)
- client үйлдлүүд ажиллахгүй (useState, useEffect)

```tsx
export default function Page() {
  const t = useTranslations("global");
  return <div>{t("ehlo world")}</div>;
}
```

### client component

үндсэн React component

- hooks ажиллана
- бүх useState, useEffect ажиллана
- animation үйлдэл хийх зэрэг үед хэрэглэнэ
- client талын (browser) дээр харагдах юмнуудыг хийх зорилгогоор ашиглана (client side form validation гэх мэт)
- заавал `"use client"` тавина.

```tsx
"use client";
export const SomeClientComponent = () => {
  const [magic, setMagic] = useState("~~~");
  useEffect(() => {
    doSomethingServerSide().then((data) => {
      setMagic(data.message);
    });
  });

  return <div>{magic}</div>;
};
```
