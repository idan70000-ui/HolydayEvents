שרשרת תהליך האוטומציה שלנו:
GitHub → Jenkins → Build/Test → Docker Image → Docker Hub → Ansible → Deployment Server → Running App



Jenkins מריץ את ansible-playbook -i inventory.ini setup.yml, תפקידו הוא להתקין את הסביבה של דוקר על השרת שלנו באנסיבל כולל את כל חבילות הבסיס הנדרשות.

Jenkins מריץ את ansible-playbook -i inventory.ini deploy.yml, אשר:
מושך את האימג העדכני ביותר מדוקר האב
עוצר ומסיר את הקונטיינר הישן, אם קיים
מריץ קונטיינר חדש מהאימג שמשכנו (הכי עדכני)
ממתין מספר שניות לעליית האפליקציה
שולח בקשה ל-endpoint GET /health ומוודא תשובת 200 עם תוכן שמכיל "healthy"
נכשל אם הבדיקה לא עוברת כדי שגנקינס ידע שהדיפלוי שעשינו לא עבד או לא הצליח......




האפליקציה שלנו רצה בצורה טובה ותקינה על השרת בפורט 3001:3000
