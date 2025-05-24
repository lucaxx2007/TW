import { useState, useEffect } from 'react';
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";

export default function Home() {
  const [beforeImage, setBeforeImage] = useState(null);
  const [afterImage, setAfterImage] = useState(null);
  const [description, setDescription] = useState("");
  const [cards, setCards] = useState([]);

  useEffect(() => {
    const saved = JSON.parse(localStorage.getItem("cards") || "[]");
    setCards(saved);
  }, []);

  const handleImageUpload = (e, setter) => {
    const file = e.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onloadend = () => setter(reader.result);
      reader.readAsDataURL(file);
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!beforeImage || !afterImage || !description) return;

    const newCard = { beforeImage, afterImage, description };
    const updatedCards = [newCard, ...cards];
    setCards(updatedCards);
    localStorage.setItem("cards", JSON.stringify(updatedCards));

    setBeforeImage(null);
    setAfterImage(null);
    setDescription("");
  };

  return (
    <div className="min-h-screen bg-[#0d1117] text-white font-sans">
      <header className="text-center py-12 bg-gradient-to-r from-cyan-500 to-blue-700 shadow-lg">
        <h1 className="text-4xl font-bold">Sandra Tricoterapeuta</h1>
        <h2 className="text-xl mt-2 font-light">Especialista em cuidados capilares e estética</h2>
      </header>

      <main className="max-w-5xl mx-auto px-4 py-8">
        <form onSubmit={handleSubmit} className="bg-[#161b22] p-6 rounded-2xl shadow-lg mb-10">
          <input
            type="file"
            accept="image/*"
            onChange={(e) => handleImageUpload(e, setBeforeImage)}
            className="mb-4 w-full bg-[#0d1117] p-3 rounded-lg border border-gray-700"
          />
          <input
            type="file"
            accept="image/*"
            onChange={(e) => handleImageUpload(e, setAfterImage)}
            className="mb-4 w-full bg-[#0d1117] p-3 rounded-lg border border-gray-700"
          />
          <textarea
            value={description}
            onChange={(e) => setDescription(e.target.value)}
            placeholder="Descreva o resultado..."
            className="mb-4 w-full bg-[#0d1117] p-3 rounded-lg border border-gray-700"
          />
          <Button className="w-full bg-cyan-600 hover:bg-cyan-500">Adicionar Resultado</Button>
        </form>

        <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
          {cards.map((card, index) => (
            <Card key={index} className="bg-[#161b22]">
              <CardContent className="p-4">
                <div className="flex gap-2 mb-2">
                  <img src={card.beforeImage} alt="Antes" className="w-1/2 rounded-lg" />
                  <img src={card.afterImage} alt="Depois" className="w-1/2 rounded-lg" />
                </div>
                <p className="text-sm text-gray-300">{card.description}</p>
              </CardContent>
            </Card>
          ))}
        </div>
      </main>
    </div>
  );
}
