# appgestion-de-rendez-vous-
using System;
using System.Collections.Generic;

namespace GestionRendezVous
{
    class Employe
    {
        public string nom;
        public List<Disponibilite> disponibilites;
        public Employe(string nom)
        {
            this.nom = nom;
            this.disponibilites = new List<Disponibilite>();
        }

        public void AjouterDisponibilite(Disponibilite disponibilite)
        {
            this.disponibilites.Add(disponibilite);
        }

        public bool EstDisponible(DateTime debut, DateTime fin)
        {
            foreach (Disponibilite disponibilite in this.disponibilites)
            {
                if (disponibilite.Debut <= debut && disponibilite.Fin >= fin)
                {
                    return true;
                }
            }

            return false;
        }
    }

    class Disponibilite
    {
        public DateTime Debut;
        public DateTime Fin;

        public Disponibilite(DateTime debut, DateTime fin)
        {
            this.Debut = debut;
            this.Fin = fin;
        }
    }

    class RendezVous
    {
        public List<Employe> employes;
        public DateTime debut;
        public DateTime fin;

        public RendezVous(DateTime debut, DateTime fin)
        {
            this.employes = new List<Employe>();
            this.debut = debut;
            this.fin = fin;
        }

        public bool EstValide()
        {
            foreach (Employe employe in this.employes)
            {
                if (!employe.EstDisponible(this.debut, this.fin))
                {
                    return false;
                }
            }

            return true;
        }
    }

    class Program
    {
        static List<Employe> employes = new List<Employe>();

        static void Main(string[] args)
        {
            // Entrer les disponibilités des employés
            AjouterDisponibilites("Alice", new List<Disponibilite> { new Disponibilite(new DateTime(2023, 4, 1, 8, 0, 0), new DateTime(2023, 4, 1, 12, 0, 0)) });
            AjouterDisponibilites("Bob", new List<Disponibilite> { new Disponibilite(new DateTime(2023, 4, 1, 13, 0, 0), new DateTime(2023, 4, 1, 17, 0, 0)) });
            AjouterDisponibilites("Charlie", new List<Disponibilite> { new Disponibilite(new DateTime(2023, 4, 2, 8, 0, 0), new DateTime(2023, 4, 2, 12, 0, 0)), new Disponibilite(new DateTime(2023, 4, 2, 13, 0, 0), new DateTime(2023, 4, 2, 17, 0, 0)) });

            // Obtenir les disponibilités pour une sélection d'employés
            List<Employe> selection = new List<Employe> { GetEmploye("Alice"), GetEmploye("Charlie") };
            DateTime debut = new DateTime(2023, 4, 1, 9, 0, 0);
            DateTime fin = new DateTime(2023, 4, 1, 10, 0, 0);
            List<Disponibilite> disponibilites = ObtenirDisponibilites(selection, debut, fin);
            // Afficher les disponibilités obtenues
            Console.WriteLine("Disponibilités pour la plage horaire {0} - {1} :", debut.ToString("g"), fin.ToString("g"));
            foreach (Disponibilite disponibilite in disponibilites)
            {
                Console.WriteLine("{0} de {1} à {2}", disponibilite.Debut.ToString("D"), disponibilite.Debut.ToString("t"), disponibilite.Fin.ToString("t"));
            }
        }

        static void AjouterDisponibilites(string nomEmploye, List<Disponibilite> disponibilites)
        {
            Employe employe = GetEmploye(nomEmploye);

            if (employe != null)
            {
                foreach (Disponibilite disponibilite in disponibilites)
                {
                    employe.AjouterDisponibilite(disponibilite);
                }
            }
        }

        static Employe GetEmploye(string nom)
        {
            Employe employe = employes.Find(e => e.nom == nom);
 if (employe == null)
            {
                employe = new Employe(nom);
                employes.Add(employe);
            }

            return employe;
        }

        static List<Disponibilite> ObtenirDisponibilites(List<Employe> employesSelectionnes, DateTime debut, DateTime fin)
        {
            List<Disponibilite> disponibilites = new List<Disponibilite>();

            // Vérifier si la plage horaire est valide pour chaque employé sélectionné
            foreach (Employe employe in employesSelectionnes)
            {
                if (employe.EstDisponible(debut, fin))
                {
                    // Ajouter la plage horaire disponible pour l'employé
                    disponibilites.Add(new Disponibilite(debut, fin));
                }
            }

            return disponibilites;
        }
    }
}
