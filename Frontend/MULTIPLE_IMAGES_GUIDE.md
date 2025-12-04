# تعليمات: إضافة صور متعددة في Admin Panel

## التعديل المطلوب في ملف `index.html`:

ابحث عن السطر:
```html
<input type="file" id="p-image-file" accept="image/*">
```

واستبدله بـ:
```html
<input type="file" id="p-image-file" accept="image/*" multiple>
<small style="color: #888; display: block; margin-top: 5px;">
    💡 يمكنك اختيار صورة واحدة أو عدة صور. الصورة الأولى ستكون الرئيسية.
</small>
<div id="image-preview" style="display: flex; gap: 10px; flex-wrap: wrap; margin-top: 10px;"></div>
```

## التعديل المطلوب في ملف `app.js`:

أضف هذا الكود بعد السطر الذي يحتوي على `document.getElementById('p-image-file')`:

```javascript
// Image preview for multiple images
document.getElementById('p-image-file').addEventListener('change', function(e) {
    const preview = document.getElementById('image-preview');
    preview.innerHTML = '';
    
    const files = Array.from(e.target.files);
    files.forEach((file, index) => {
        const reader = new FileReader();
        reader.onload = function(event) {
            const img = document.createElement('img');
            img.src = event.target.result;
            img.style.width = '100px';
            img.style.height = '100px';
            img.style.objectFit = 'cover';
            img.style.borderRadius = '8px';
            img.style.border = index === 0 ? '3px solid #d4af37' : '2px solid #555';
            img.title = index === 0 ? 'الصورة الرئيسية' : `صورة ${index + 1}`;
            preview.appendChild(img);
        };
        reader.readAsDataURL(file);
    });
});
```

## الخطوة النهائية:

في دالة رفع الصورة، عدّل الكود ليرفع جميع الصور ويفصل بينها بفاصلة:

```javascript
const imageUrls = [];
for (let file of files) {
    const url = await uploadToCloudinary(file);
    imageUrls.push(url);
}
const finalImageUrl = imageUrls.join(',');
```

---

## ✅ النتيجة:
- يمكن اختيار صور متعددة
- معاينة الصور قبل الرفع
- الصورة الأولى لها إطار ذهبي (الرئيسية)
- يتم حفظ الروابط مفصولة بفاصلة في قاعدة البيانات
