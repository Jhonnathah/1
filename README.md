import { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Code, Linkedin } from "lucide-react";

const GITHUB_USERNAME = "seu-usuario-github";
const LINKEDIN_URL = "https://www.linkedin.com/in/seu-perfil";

export default function Portfolio() {
  const [repos, setRepos] = useState([]);
  const [selectedRepo, setSelectedRepo] = useState(null);

  useEffect(() => {
    fetch(`https://api.github.com/users/${GITHUB_USERNAME}/repos`)
      .then(response => response.json())
      .then(data => setRepos(data))
      .catch(error => console.error("Erro ao buscar repositórios:", error));
  }, []);

  return (
    <div className="p-8 max-w-4xl mx-auto text-center">
      <h1 className="text-3xl font-bold mb-4">Olá, sou um Analista de Sistemas</h1>
      <p className="text-lg text-gray-600 mb-6">Desenvolvedor apaixonado por tecnologia, construindo soluções inovadoras.</p>
      
      <a href={LINKEDIN_URL} target="_blank" rel="noopener noreferrer" className="inline-flex items-center px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 mb-6">
        <Linkedin className="mr-2" /> Conecte-se comigo no LinkedIn
      </a>
      
      <h2 className="text-2xl font-semibold mb-4">Meus Projetos no GitHub</h2>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
        {repos.map((repo, index) => (
          <Card key={index} className="p-4">
            <CardContent>
              <h2 className="text-xl font-semibold">{repo.name}</h2>
              <p className="text-sm text-gray-600">{repo.description || "Sem descrição"}</p>
              <Button className="mt-2" onClick={() => setSelectedRepo(repo)}>
                <Code className="mr-2" /> Ver Código
              </Button>
            </CardContent>
          </Card>
        ))}
      </div>

      {selectedRepo && (
        <div className="mt-6 p-4 border rounded-lg bg-gray-100">
          <h3 className="text-lg font-bold">Código de {selectedRepo.name}</h3>
          <p className="mb-2">{selectedRepo.description}</p>
          <a href={selectedRepo.html_url} target="_blank" rel="noopener noreferrer" className="text-blue-500 underline">
            Acessar repositório no GitHub
          </a>
        </div>
      )}
    </div>
  );
}
