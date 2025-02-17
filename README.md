from IPython.display import display, HTML

html_code = '''
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Inscription - Collège de Diade</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background-color: #f0f8ff;
        }

        .form-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0,0,0,0.1);
        }

        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 30px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #34495e;
            font-weight: bold;
        }

        input, select, textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #bdc3c7;
            border-radius: 5px;
            box-sizing: border-box;
        }

        input:focus, select:focus, textarea:focus {
            outline: 2px solid #3498db;
        }

        .grid-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        button {
            background-color: #3498db;
            color: white;
            padding: 12px 25px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #2980b9;
        }

        .required::after {
            content: "*";
            color: red;
            margin-left: 3px;
        }
    </style>
</head>
<body>
    <div class="form-container">
        <h1>Formulaire d'Inscription<br><span style="font-size: 0.8em;">Collège de Diade</span></h1>
        
        <form id="inscriptionForm" onsubmit="return validateForm(event)">
            <div class="grid-container">
                <div class="form-group">
                    <label class="required">Prénom</label>
                    <input type="text" id="prenom" required>
                </div>
                
                <div class="form-group">
                    <label class="required">Nom</label>
                    <input type="text" id="nom" required>
                </div>
            </div>

            <div class="form-group">
                <label class="required">Date de naissance</label>
                <input type="date" id="naissance" required>
            </div>

            <div class="form-group">
                <label class="required">Genre</label>
                <select id="genre" required>
                    <option value="">Sélectionner...</option>
                    <option>Masculin</option>
                    <option>Féminin</option>
                    <option>Autre</option>
                </select>
            </div>

            <div class="form-group">
                <label class="required">Email</label>
                <input type="email" id="email" required>
            </div>

            <div class="form-group">
                <label class="required">Téléphone</label>
                <input type="tel" id="telephone" pattern="[0-9]{10}" required>
            </div>

            <div class="form-group">
                <label>Adresse</label>
                <textarea id="adresse" rows="3"></textarea>
            </div>

            <div class="form-group">
                <label class="required">Filière choisie</label>
                <select id="filiere" required>
                    <option value="">Sélectionner une filière...</option>
                    <option>Sciences</option>
                    <option>Lettres</option>
                    <option>Commerce</option>
                    <option>Ingénierie</option>
                </select>
            </div>

            <button type="submit">Soumettre l'inscription</button>
        </form>
    </div>

    <script>
        function validateForm(event) {
            event.preventDefault();

            // Vérification des champs requis
            const requiredFields = document.querySelectorAll('[required]');
            let isValid = true;

            requiredFields.forEach(field => {
                if (!field.value.trim()) {
                    isValid = false;
                    field.style.outline = '2px solid red';
                } else {
                    field.style.outline = '';
                }
            });

            // Vérification spécifique pour l'email
            const email = document.getElementById('email');
            if (!/\S+@\S+\.\S+/.test(email.value)) {
                isValid = false;
                email.style.outline = '2px solid red';
            }

            if (isValid) {
                // Récupération des données
                const formData = {
                    prenom: document.getElementById('prenom').value,
                    nom: document.getElementById('nom').value,
                    naissance: document.getElementById('naissance').value,
                    genre: document.getElementById('genre').value,
                    email: document.getElementById('email').value,
                    telephone: document.getElementById('telephone').value,
                    adresse: document.getElementById('adresse').value,
                    filiere: document.getElementById('filiere').value
                };

                // Affichage des résultats
                alert('Inscription réussie !\n\n' + 
                      `Prénom: ${formData.prenom}\n` +
                      `Nom: ${formData.nom}\n` +
                      `Filière: ${formData.filiere}\n` +
                      `Email: ${formData.email}`);
                
                // Réinitialisation du formulaire
                document.getElementById('inscriptionForm').reset();
                return true;
            } else {
                alert('Veuillez remplir correctement tous les champs requis !');
                return false;
            }
        }
    </script>
</body>
</html>
'''

display(HTML(html_code)) 
