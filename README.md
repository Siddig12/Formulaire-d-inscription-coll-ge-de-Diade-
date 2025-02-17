<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Formulaire d'inscription en ligne - Collège de Diade">
    <title>Inscription - Collège de Diade</title>
    <style>
        :root {
            --primary-color: #2c3e50;
            --secondary-color: #3498db;
            --error-color: #e74c3c;
        }

        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 20px;
            background: #f8f9fa;
        }

        .container {
            max-width: 800px;
            margin: 2rem auto;
            background: white;
            padding: 2rem;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        h1 {
            color: var(--primary-color);
            text-align: center;
            margin-bottom: 2rem;
        }

        .form-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1.5rem;
            margin-bottom: 1.5rem;
        }

        .form-group {
            margin-bottom: 1rem;
        }

        label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: var(--primary-color);
        }

        .required::after {
            content: " *";
            color: var(--error-color);
        }

        input, select, textarea {
            width: 100%;
            padding: 0.8rem;
            border: 2px solid #ddd;
            border-radius: 6px;
            transition: border-color 0.3s ease;
        }

        input:focus, select:focus, textarea:focus {
            border-color: var(--secondary-color);
            outline: none;
        }

        button {
            background: var(--secondary-color);
            color: white;
            padding: 1rem 2rem;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1.1rem;
            width: 100%;
            transition: transform 0.2s, background 0.3s;
        }

        button:hover {
            background: #2980b9;
            transform: translateY(-2px);
        }

        .loading {
            position: relative;
            pointer-events: none;
        }

        .loading::after {
            content: "";
            position: absolute;
            right: 1rem;
            top: 50%;
            transform: translateY(-50%);
            width: 1.5rem;
            height: 1.5rem;
            border: 3px solid rgba(255,255,255,0.3);
            border-top-color: white;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            to { transform: translateY(-50%) rotate(360deg); }
        }

        @media (max-width: 480px) {
            .container {
                margin: 1rem;
                padding: 1rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Inscription au Collège de Diade</h1>
        <form id="inscriptionForm">
            <div class="form-grid">
                <div class="form-group">
                    <label class="required">Prénom</label>
                    <input type="text" id="prenom" required>
                </div>
                <div class="form-group">
                    <label class="required">Nom</label>
                    <input type="text" id="nom" required>
                </div>
            </div>

            <div class="form-grid">
                <div class="form-group">
                    <label class="required">Date de naissance</label>
                    <input type="date" id="naissance" required>
                </div>
                <div class="form-group">
                    <label class="required">Genre</label>
                    <select id="genre" required>
                        <option value="">Choisir...</option>
                        <option>Masculin</option>
                        <option>Féminin</option>
                        <option>Autre</option>
                    </select>
                </div>
            </div>

            <div class="form-grid">
                <div class="form-group">
                    <label class="required">Email</label>
                    <input type="email" id="email" required>
                </div>
                <div class="form-group">
                    <label class="required">Téléphone</label>
                    <input type="tel" id="telephone" pattern="[0-9]{10}" required>
                </div>
            </div>

            <div class="form-group">
                <label>Adresse complète</label>
                <textarea id="adresse" rows="3"></textarea>
            </div>

            <div class="form-group">
                <label class="required">Filière</label>
                <select id="filiere" required>
                    <option value="">Choisir une filière</option>
                    <option>Sciences</option>
                    <option>Lettres</option>
                    <option>Commerce</option>
                    <option>Informatique</option>
                </select>
            </div>

            <button type="submit">Soumettre l'inscription</button>
        </form>
    </div>

    <script>
        const form = document.getElementById('inscriptionForm');
        const btnSubmit = form.querySelector('button');

        form.addEventListener('submit', async (e) => {
            e.preventDefault();
            btnSubmit.classList.add('loading');
            
            if(validateForm()) {
                const formData = {
                    timestamp: new Date().toISOString(),
                    prenom: document.getElementById('prenom').value.trim(),
                    nom: document.getElementById('nom').value.trim(),
                    naissance: document.getElementById('naissance').value,
                    genre: document.getElementById('genre').value,
                    email: document.getElementById('email').value.trim(),
                    telephone: document.getElementById('telephone').value.trim(),
                    adresse: document.getElementById('adresse').value.trim(),
                    filiere: document.getElementById('filiere').value
                };

                try {
                    // Remplacez par votre URL Google Apps Script
                    const response = await fetch('https://script.google.com/macros/s/ABCDEF/exec', {
                        method: 'POST',
                        body: JSON.stringify(formData),
                        headers: { 'Content-Type': 'application/json' }
                    });

                    if(response.ok) {
                        showConfirmation();
                        form.reset();
                    }
                } catch(error) {
                    console.error('Erreur:', error);
                    alert('Une erreur est survenue. Veuillez réessayer.');
                }
            }
            
            btnSubmit.classList.remove('loading');
        });

        function validateForm() {
            let isValid = true;
            const requiredFields = form.querySelectorAll('[required]');

            requiredFields.forEach(field => {
                field.style.borderColor = '#ddd';
                
                if(!field.value.trim()) {
                    isValid = false;
                    field.style.borderColor = 'var(--error-color)';
                }
                
                if(field.type === 'email' && !isValidEmail(field.value)) {
                    isValid = false;
                    field.style.borderColor = 'var(--error-color)';
                }
            });

            return isValid;
        }

        function isValidEmail(email) {
            return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
        }

        function showConfirmation() {
            const confirmation = document.createElement('div');
            confirmation.style = `
                position: fixed;
                top: 20px;
                right: 20px;
                background: #2ecc71;
                color: white;
                padding: 1rem 2rem;
                border-radius: 6px;
                box-shadow: 0 3px 6px rgba(0,0,0,0.1);
                animation: slideIn 0.3s ease-out;
            `;
            
            confirmation.textContent = 'Inscription réussie !';
            document.body.appendChild(confirmation);

            setTimeout(() => {
                confirmation.style.animation = 'slideOut 0.3s ease-in';
                setTimeout(() => confirmation.remove(), 300);
            }, 3000);
        }

        // Ajouter les animations CSS dynamiquement
        const style = document.createElement('style');
        style.textContent = `
            @keyframes slideIn {
                from { transform: translateX(100%); }
                to { transform: translateX(0); }
            }
            @keyframes slideOut {
                from { transform: translateX(0); }
                to { transform: translateX(100%); }
            }
        `;
        document.head.appendChild(style);
    </script>
</body>
</html>
