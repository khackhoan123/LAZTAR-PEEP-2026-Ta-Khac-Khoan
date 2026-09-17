+++
title = "Day 02 - 16/09/2026 (Remote)"
weight = 2
+++

## PHẦN 1. REACT CƠ BẢN

### 1. React là gì?
React là thư viện JavaScript mã nguồn mở do Meta (Facebook) phát triển, chuyên dùng để xây dựng giao diện người dùng (UI), đặc biệt là các ứng dụng đơn trang (Single Page Applications - SPA). React chỉ tập trung vào tầng View (trong mô hình MVC) và vận hành theo kiến trúc Component-Driven.

### 2. Component trong React là gì? Có mấy loại component?
Component là một khối giao diện độc lập, có thể tái sử dụng và tự quản lý logic/trạng thái hiển thị riêng biệt. Trong React có 2 loại chính:
- **Function Component:** Khai báo dưới dạng hàm JavaScript nhận props và trả về JSX. Đây là chuẩn phát triển chính thức hiện nay kết hợp với React Hooks.
- **Class Component:** Khai báo bằng class kế thừa từ `React.Component`, quản lý trạng thái bằng `this.state` và các phương thức vòng đời (*lifecycle methods*). Hiện ít được sử dụng trong các dự án mới.

### 3. JSX là gì?
JSX (*JavaScript XML*) là cú pháp mở rộng của JavaScript, cho phép viết các thẻ giao diện tương tự HTML trực tiếp bên trong mã JS. Trình biên dịch (Babel hoặc SWC) sẽ tự động biên dịch JSX thành các lệnh gọi hàm thuần `React.createElement()`.

### 4. Props là gì?
Props (*Properties*) là phương thức truyền dữ liệu một chiều từ component cha xuống component con. Props có tính chất **bất biến (read-only)**; component con nhận props không được quyền trực tiếp chỉnh sửa giá trị bên trong.

### 5. State là gì? State khác Props như thế nào?
State là đối tượng dữ liệu nội bộ được lưu trữ và quản lý bên trong một component, có thể thay đổi theo thời gian qua tương tác của người dùng hoặc phản hồi từ API. Khi state thay đổi, component sẽ tự động re-render để cập nhật giao diện.

| Tiêu chí | Props | State |
| :--- | :--- | :--- |
| **Nguồn gốc** | Truyền từ component cha xuống | Khởi tạo và quản lý nội bộ component |
| **Khả năng sửa đổi** | Bất biến (Read-only) | Có thể cập nhật (thông qua hàm setState) |
| **Mục đích** | Tái sử dụng và cấu hình component con | Quản lý dữ liệu động và hành vi nội bộ |

### 6. Virtual DOM là gì? Vì sao React sử dụng Virtual DOM?
Virtual DOM (VDOM) là một bản sao ảo dạng cây đối tượng JavaScript đại diện cho cây Real DOM thật của trình duyệt.
- **Lý do sử dụng:** Can thiệp trực tiếp vào Real DOM rất tốn tài nguyên. Khi dữ liệu thay đổi, React dựng một cây VDOM mới, chạy thuật toán so khớp (*Diffing Algorithm* / Reconciliation) với cây VDOM cũ để tìm ra các điểm khác biệt, sau đó chỉ cập nhật đúng các node thay đổi lên Real DOM (*Batch update*), giúp tối ưu hiệu năng hiển thị.

### 7. Hooks là gì? Kể tên một số Hook phổ biến trong React.
Hooks là các hàm đặc biệt được giới thiệu từ React 16.8, cho phép Function Component sử dụng State, quản lý vòng đời và các tính năng nâng cao khác mà không cần viết Class Component.
- **Các Hook phổ biến:** `useState`, `useEffect`, `useContext`, `useRef`, `useMemo`, `useCallback`, `useReducer`.

### 8. `useState` dùng để làm gì?
`useState` dùng để khai báo và theo dõi biến trạng thái (state) nội bộ trong Function Component. Cú pháp: `const [state, setState] = useState(initialValue);`.

### 9. `useEffect` dùng để làm gì?
`useEffect` dùng để xử lý các tác vụ phụ (*Side Effects*) trong Function Component, bao gồm: gọi API nạp dữ liệu, đăng ký/hủy sự kiện trình duyệt, thao tác DOM thủ công, hoặc thiết lập timer (`setTimeout`/`setInterval`).

### 10. Lifecycle của một React Component gồm những giai đoạn nào?
Vòng đời component trải qua 3 giai đoạn chính:
- **Mounting:** Khởi tạo và gắn component vào cây Real DOM lần đầu tiên.
- **Updating:** Re-render lại khi component nhận props mới hoặc state nội bộ thay đổi.
- **Unmounting:** Loại bỏ component ra khỏi cây Real DOM và dọn dẹp bộ nhớ/sự kiện liên quan.

### 11. Client-Side Rendering (CSR) là gì?
CSR là mô hình kết xuất giao diện tại trình duyệt máy khách (Client). Server chỉ phản hồi một file HTML ban đầu gần như rỗng (kèm thẻ `<div id="root"></div>`) và các file JavaScript. Trình duyệt tải JS về, thực thi và tự vẽ toàn bộ cấu trúc dữ liệu, giao diện lên màn hình.

### 12. React Router là gì?
React Router là thư viện định tuyến tiêu chuẩn cho React, cho phép chuyển đổi qua lại giữa các view/giao diện trong ứng dụng Single Page Application (SPA) mà không cần tải lại toàn bộ trang từ máy chủ.

### 13. React thuần có hỗ trợ Routing, SEO và API Server không?
- **Routing:** Không hỗ trợ sẵn (cần cài đặt thư viện ngoài như `react-router-dom`).
- **SEO:** Mặc định rất kém vì ban đầu HTML trả về trắng tinh, các web crawler khó đọc được nội dung trước khi JavaScript hoàn tất thực thi.
- **API Server:** Hoàn toàn không hỗ trợ; React thuần túy hoạt động ở client và cần hệ thống backend độc lập.

### 14. Context API là gì? Khi nào nên sử dụng?
Context API là giải pháp tích hợp sẵn của React giúp truyền dữ liệu trực tiếp xuyên qua cây component mà không cần thông qua các cấp trung gian (*tránh hiện tượng Prop Drilling*).
- **Nên dùng khi:** Chia sẻ dữ liệu mang tính toàn cục (Global State) như theme (Light/Dark mode), trạng thái đăng nhập người dùng (Auth status), hoặc cấu hình đa ngôn ngữ (i18n).

### 15. SPA (Single Page Application) là gì?
SPA là kiến trúc ứng dụng web chỉ tải một trang HTML duy nhất từ máy chủ trong lần truy cập đầu tiên. Khi người dùng thao tác hoặc chuyển trang, ứng dụng chỉ tải dữ liệu thô (JSON) thông qua API và cập nhật lại phần giao diện cần thiết, mang lại trải nghiệm mượt mà tương tự phần mềm desktop.

---

## PHẦN 2. SO SÁNH REACT VÀ NEXT.JS

### 1. Next.js là gì?
Next.js là một React Framework hoàn chỉnh do Vercel phát triển, cung cấp các giải pháp tối ưu hóa hiệu năng, thân thiện với SEO thông qua nhiều cơ chế kết xuất linh hoạt (SSR, SSG, ISR) cùng kiến trúc hỗ trợ full-stack.

### 2. Điểm khác biệt cốt lõi giữa React và Next.js là gì?
- **React:** Là một **thư viện UI (Library)** thuần túy, chỉ tập trung giải quyết tầng giao diện ở Client (CSR). Lập trình viên phải tự tích hợp routing, bundle, tối ưu SEO và máy chủ.
- **Next.js:** Là một **khung kiến trúc hoàn chỉnh (Framework)** xây dựng phía trên React, tích hợp sẵn định tuyến tập tin (*File-based routing*), tối ưu tài nguyên ảnh/font, các cơ chế render phía server và hệ thống API Route.

### 3. Routing trong React và Next.js khác nhau như thế nào?
- **React:** Định tuyến bằng mã thông qua thư viện bên ngoài (`react-router-dom`).
- **Next.js:** Định tuyến hoàn toàn theo cấu trúc thư mục và tập tin (**File-based Routing**). Một thư mục kèm file `page.tsx` (như `app/about/page.tsx`) sẽ tự động map thành route `/about`.

### 4. Rendering trong React và Next.js khác nhau ra sao?
- **React:** Mặc định chỉ kết xuất ở phía máy khách (**CSR**).
- **Next.js:** Hỗ trợ linh hoạt nhiều kỹ thuật kết xuất: **SSR** (kết xuất trên server theo từng request), **SSG** (tạo file tĩnh lúc build), **ISR** (cập nhật trang tĩnh định kỳ ngầm), và **CSR** (Client Component).

### 5. Vì sao Next.js hỗ trợ SEO tốt hơn React thuần?
Next.js tạo sẵn mã HTML hoàn chỉnh chứa đầy đủ nội dung và thẻ meta ngay trên máy chủ trước khi trả về máy khách. Các bot tìm kiếm (Google, Bing) và mạng xã hội có thể đọc trực tiếp dữ liệu tĩnh này ngay lập tức mà không cần phụ thuộc vào việc thực thi mã JavaScript.

### 6. Hiệu năng tải trang đầu tiên (First Load) của React và Next.js khác nhau như thế nào?
- **React (CSR):** Tải trang đầu thường chậm hơn do trình duyệt phải tải toàn bộ file bundle JS lớn về rồi mới bắt đầu dựng layout (dễ gây tình trạng màn hình trắng ban đầu).
- **Next.js (SSR/SSG):** Tải trang đầu rất nhanh vì máy khách nhận được ngay mã HTML có sẵn nội dung để hiển thị tức thì (*chỉ số First Contentful Paint - FCP rất tốt*).

### 7. Cấu trúc dự án React và Next.js khác nhau ra sao?
- **React (Vite/CRA):** Tự do, cấu hình mở, điểm bắt đầu là file `index.html` và `src/App.tsx`.
- **Next.js:** Tuân thủ quy chuẩn nghiêm ngặt theo quy ước (nhất là trong App Router): dùng thư mục `app/`, kèm các tệp định danh quy ước như `layout.tsx`, `page.tsx`, `loading.tsx`, `error.tsx`, `route.ts`.

### 8. Next.js có thay thế React không? Vì sao?
Không. Next.js được xây dựng trực tiếp **trên nền tảng của React**. Mọi component viết trong Next.js vẫn sử dụng cú pháp JSX và tư duy component của React. Next.js đóng vai trò bộ khung mở rộng giải quyết các bài toán về routing, SEO, server rendering và deployment.

### 9. Khi nào nên dùng React thuần và khi nào nên dùng Next.js?
- **Nên dùng React thuần:** Các trang quản trị nội bộ (Admin Dashboard), ứng dụng web doanh nghiệp cần đăng nhập bảo mật, hoặc hệ thống phức tạp không cần hiển thị công khai trên công cụ tìm kiếm.
- **Nên dùng Next.js:** Các trang thương mại điện tử (E-commerce), trang tin tức, blog cá nhân, Landing Page, hoặc bất kỳ hệ thống nào ưu tiên chỉ số SEO và tốc độ tải trang lần đầu.

---

## PHẦN 3. NEXT.JS CHUYÊN SÂU

### 1. App Router và Pages Router trong Next.js là gì?
- **Pages Router (cũ):** Định tuyến dựa trên các file trong thư mục `pages/`; nạp dữ liệu thông qua các hàm như `getStaticProps`, `getServerSideProps`.
- **App Router (mới - Next.js 13+):** Định tuyến dựa trên thư mục `app/`, hỗ trợ cấu trúc lồng nhau (*nested layouts*), mặc định chạy kiến trúc **React Server Components (RSC)** và tích hợp streaming UI hiện đại.

### 2. Server Component và Client Component khác nhau như thế nào?
- **Server Component (mặc định trong App Router):** Được biên dịch và chạy hoàn toàn trên server, không gửi mã JS về trình duyệt, không dùng được hook tương tác (`useState`, `useEffect`) hay sự kiện DOM (`onClick`). Giúp giảm dung lượng bundle và bảo vệ khóa API an toàn.
- **Client Component:** Bắt buộc khai báo chỉ thị `'use client'` ở dòng đầu tiên của file. Chạy tại trình duyệt máy khách, hỗ trợ đầy đủ các React Hooks và tương tác của người dùng.

### 3. SSR (Server-Side Rendering) là gì?
Là phương pháp mà mã HTML của trang web được biên dịch và tạo mới trên máy chủ **mỗi khi có yêu cầu (request)** gửi đến từ người dùng, sau đó mới trả về trình duyệt kèm dữ liệu mới nhất.

### 4. SSG (Static Site Generation) là gì?
Là phương pháp mà toàn bộ mã HTML của các trang được xuất tĩnh một lần duy nhất **ngay tại thời điểm build dự án (Build Time)**. Các file HTML tĩnh này sau đó được lưu trữ trên mạng phân phối nội dung (CDN) để phục vụ lập tức cho người truy cập.

### 5. ISR (Incremental Static Regeneration) là gì?
Là cơ chế cho phép tái tạo hoặc cập nhật lại các trang tĩnh (SSG) ngầm ở phía nền theo chu kỳ thời gian được định trước (thông qua thuộc tính `revalidate`) mà không cần phải chạy lại toàn bộ tiến trình build dự án.

### 6. File-based Routing trong Next.js hoạt động như thế nào?
Hệ thống chuyển đổi trực tiếp cấu trúc thư mục vật lý thành URL:
- Thư mục `app/projects/page.tsx` sẽ tương ứng với đường dẫn `/projects`.
- Mỗi cấp thư mục con đóng vai trò một phân đoạn đường dẫn (*route segment*), và tập tin `page.tsx` xác định nội dung được hiển thị cho phân đoạn đó.

### 7. Dynamic Route trong Next.js là gì?
Là cơ chế định tuyến khi đường dẫn phụ thuộc vào dữ liệu biến đổi (như ID hoặc slug). Trong App Router, dynamic route được định nghĩa bằng tên thư mục đặt trong cặp dấu ngoặc vuông (ví dụ: `app/blog/[slug]/page.tsx`).

### 8. `layout.tsx` trong App Router dùng để làm gì?
`layout.tsx` định nghĩa bộ khung giao diện chung được chia sẻ giữa nhiều trang (như Navbar, Footer). Layout duy trì trạng thái dữ liệu (*state*), giữ nguyên tính tương tác và không bị re-render lại khi người dùng chuyển hướng giữa các trang con lồng bên trong.

### 9. API Routes (Route Handlers) trong Next.js là gì?
Là tính năng cho phép xây dựng các điểm cuối Backend (RESTful API) trực tiếp trong ứng dụng Next.js. Trong App Router, API được viết trong file `route.ts` với các hàm HTTP tiêu chuẩn như `GET`, `POST`, `PUT`, `DELETE`.

### 10. `getStaticProps` và `getServerSideProps` là gì? Chúng dùng trong trường hợp nào?
Đây là hai phương thức nạp dữ liệu thuộc kiến trúc **Pages Router**:
- `getStaticProps`: Lấy dữ liệu tĩnh tại thời điểm build (dùng cho SSG).
- `getServerSideProps`: Lấy dữ liệu động trên mỗi lượt request từ client (dùng cho SSR).

*(Lưu ý: Trong App Router hiện đại, hai hàm này đã được thay thế bằng cú pháp `async/await fetch()` trực tiếp ngay trong Server Component).*

### 11. `next/image` giúp tối ưu hình ảnh như thế nào?
Component `<Image/>` của Next.js tự động tối ưu:
- Nén dung lượng và tự chuyển đổi sang định dạng ảnh hiện đại (WebP, AVIF).
- Tự động co giãn kích thước ảnh linh hoạt theo màn hình hiển thị (*responsive*).
- Tự động bật tải trễ (*Lazy loading*).
- Chống hiện tượng giật vỡ khung hình (*Cumulative Layout Shift - CLS*) nhờ bắt buộc xác định kích thước ban đầu.

### 12. Middleware trong Next.js là gì?
Middleware là đoạn mã (khai báo tại `middleware.ts`) chạy trước khi một request được hoàn tất. Tính năng này thường dùng để kiểm tra xác thực người dùng (*authentication*), chuyển hướng (*redirect*), hoặc can thiệp header/cookie của yêu cầu.

### 13. Làm thế nào để điều hướng giữa các trang trong Next.js?
- **Điều hướng giao diện:** Dùng thẻ `<Link href="...">` từ module `next/link` để điều hướng mượt mà không tải lại trang và tự động nạp trước mã nguồn (*prefetching*).
- **Điều hướng bằng mã logic:** Sử dụng hook `useRouter` từ module `next/navigation` và gọi hàm `router.push('/duong-dan')`.

### 14. Metadata và SEO trong Next.js được xử lý như thế nào?
Trong App Router, Next.js hỗ trợ quản lý cấu hình SEO tập trung:
- Đối tượng tĩnh: `export const metadata: Metadata = { title: "...", description: "..." }` trong `layout.tsx` hoặc `page.tsx`.
- Hàm động: `export async function generateMetadata(...)` để lấy tiêu đề và thẻ OpenGraph linh hoạt từ API.

### 15. Next.js có hỗ trợ TypeScript không?
Có, Next.js hỗ trợ TypeScript ngay từ bước khởi tạo dự án. Hệ thống tích hợp sẵn các bộ định nghĩa kiểu cho layout, page props, metadata và route handlers, giúp phát hiện lỗi kiểu dữ liệu trực tiếp trong quá trình phát triển.

### 16. Có thể deploy dự án Next.js lên những nền tảng nào?
- **Vercel:** Nền tảng mẹ hỗ trợ tối ưu nhất cho Next.js với CI/CD và CDN phân tán tự động.
- **Nền tảng đám mây/PaaS:** Netlify, AWS Amplify, Render, Railway.
- **Máy chủ độc lập (Self-hosted):** Đóng gói thành container Docker và chạy trên các máy chủ ảo VPS (Ubuntu/Debian) hoặc cụm Kubernetes.