# Portal de Treinamento Fiscal
import React, { useState } from 'react';
import './App.css'; // O Vite cria este arquivo automaticamente para estilização básica

export default function App() {
  const [moduloAtivo, setModuloAtivo] = useState(1);
  const [icmsLiquido, setIcmsLiquido] = useState(80);
  const [icmsAliquota, setIcmsAliquota] = useState(20);
  const [issValor, setIssValor] = useState(1000);
  const [issAliquota, setIssAliquota] = useState(5);

  const modulosPDF = [
    { id: 1, titulo: "Matriz de Incidência do ICMS" },
    { id: 2, titulo: "Tipologias e Modalidades de ICMS" },
    { id: 3, titulo: "ICMS no Amazonas e ZFM" },
    { id: 4, titulo: "O ISS e suas Modalidades" },
    { id: 5, titulo: "Legislação Municipal de Manaus" },
    { id: 6, titulo: "Práticas de Gestão de Estoque" },
    { id: 7, titulo: "A Reforma Tributária (IBS e CBS)" },
    { id: 8, titulo: "Guia Prático e Exercícios" },
    { id: 9, titulo: "Glossário Técnico" },
  ];

  const calcularICMSPorDentro = () => {
    const liquido = parseFloat(icmsLiquido) || 0;
    const aliquotaDecimal = (parseFloat(icmsAliquota) || 0) / 100;
    if (aliquotaDecimal >= 1) return { base: 0, imposto: 0 };
    const baseCalculo = liquido / (1 - aliquotaDecimal);
    const impostoDevido = baseCalculo - liquido;
    return { base: baseCalculo.toFixed(2), imposto: impostoDevido.toFixed(2) };
  };

  const calcularISS = () => {
    const valorServico = parseFloat(issValor) || 0;
    const aliquotaDecimal = (parseFloat(issAliquota) || 0) / 100;
    return (valorServico * aliquotaDecimal).toFixed(2);
  };

  const resultadoICMS = calcularICMSPorDentro();
  const resultadoISS = calcularISS();

  return (
    <div style={{ fontFamily: 'sans-serif', backgroundColor: '#f9fafb', minHeight: '100vh', padding: '20px' }}>
      
      {/* Header */}
      <header style={{ backgroundColor: '#1e3a8a', color: 'white', padding: '20px', borderRadius: '8px', marginBottom: '20px' }}>
        <h1 style={{ margin: 0 }}>Portal de Treinamento Contábil</h1>
        <p style={{ margin: '5px 0 0 0', fontSize: '14px', color: '#bfdbfe' }}>Capacitação em ICMS, ISS e Gestão Tributária</p>
      </header>

      <div style={{ display: 'flex', gap: '20px', flexWrap: 'wrap' }}>
        
        {/* Menu Lateral */}
        <aside style={{ flex: '1 1 250px', backgroundColor: 'white', padding: '20px', borderRadius: '8px', border: '1px solid #e5e7eb' }}>
          <h2 style={{ color: '#1e3a8a', fontSize: '18px', borderBottom: '2px solid #e5e7eb', paddingBottom: '10px' }}>Índice</h2>
          <ul style={{ listStyle: 'none', padding: 0 }}>
            {modulosPDF.map((mod) => (
              <li key={mod.id} style={{ marginBottom: '10px' }}>
                <button
                  onClick={() => setModuloAtivo(mod.id)}
                  style={{
                    width: '100%', textAlign: 'left', padding: '10px', borderRadius: '6px', border: 'none',
                    cursor: 'pointer', fontWeight: moduloAtivo === mod.id ? 'bold' : 'normal',
                    backgroundColor: moduloAtivo === mod.id ? '#eff6ff' : 'transparent',
                    color: moduloAtivo === mod.id ? '#1e40af' : '#4b5563'
                  }}
                >
                  Módulo {mod.id}: {mod.titulo}
                </button>
              </li>
            ))}
          </ul>
        </aside>

        {/* Conteúdo Principal e Calculadoras */}
        <main style={{ flex: '3 1 500px', display: 'flex', flexDirection: 'column', gap: '20px' }}>
          
          <section style={{ backgroundColor: 'white', padding: '20px', borderRadius: '8px', border: '1px solid #e5e7eb' }}>
            <h3 style={{ color: '#1f2937', marginTop: 0 }}>🛠️ Simuladores Fiscais</h3>
            <div style={{ display: 'flex', gap: '20px', flexWrap: 'wrap' }}>
              
              {/* Calculadora ICMS */}
              <div style={{ flex: 1, backgroundColor: '#f3f4f6', padding: '15px', borderRadius: '6px' }}>
                <h4 style={{ margin: '0 0 10px 0', color: '#1e3a8a' }}>ICMS ("Por Dentro")</h4>
                <label style={{ display: 'block', fontSize: '14px', marginBottom: '5px' }}>Valor Líquido (R$):</label>
                <input type="number" value={icmsLiquido} onChange={(e) => setIcmsLiquido(e.target.value)} style={{ width: '100%', padding: '8px', marginBottom: '10px' }} />
                
                <label style={{ display: 'block', fontSize: '14px', marginBottom: '5px' }}>Alíquota Estadual (%):</label>
                <input type="number" value={icmsAliquota} onChange={(e) => setIcmsAliquota(e.target.value)} style={{ width: '100%', padding: '8px' }} />
                
                <div style={{ marginTop: '15px', padding: '10px', backgroundColor: '#dbeafe', borderRadius: '4px', color: '#1e40af' }}>
                  <p style={{ margin: '0 0 5px 0' }}>Base: <strong>R$ {resultadoICMS.base}</strong></p>
                  <p style={{ margin: 0 }}>ICMS a Recolher: <strong>R$ {resultadoICMS.imposto}</strong></p>
                </div>
              </div>

              {/* Calculadora ISS */}
              <div style={{ flex: 1, backgroundColor: '#f3f4f6', padding: '15px', borderRadius: '6px' }}>
                <h4 style={{ margin: '0 0 10px 0', color: '#1e3a8a' }}>ISS (Serviços)</h4>
                <label style={{ display: 'block', fontSize: '14px', marginBottom: '5px' }}>Valor do Serviço (R$):</label>
                <input type="number" value={issValor} onChange={(e) => setIssValor(e.target.value)} style={{ width: '100%', padding: '8px', marginBottom: '10px' }} />
                
                <label style={{ display: 'block', fontSize: '14px', marginBottom: '5px' }}>Alíquota Municipal (%):</label>
                <input type="number" value={issAliquota} onChange={(e) => setIssAliquota(e.target.value)} style={{ width: '100%', padding: '8px' }} />
                
                <div style={{ marginTop: '15px', padding: '10px', backgroundColor: '#dcfce7', borderRadius: '4px', color: '#166534' }}>
                  <p style={{ margin: 0 }}>ISS a Recolher: <strong>R$ {resultadoISS}</strong></p>
                </div>
              </div>

            </div>
          </section>

          <section style={{ backgroundColor: 'white', padding: '30px', borderRadius: '8px', border: '1px solid #e5e7eb', minHeight: '300px' }}>
            <h2 style={{ marginTop: 0, color: '#1f2937' }}>Módulo {moduloAtivo}: {modulosPDF.find(m => m.id === moduloAtivo)?.titulo}</h2>
            <hr style={{ border: 'none', borderBottom: '1px solid #e5e7eb', marginBottom: '20px' }} />
            <p style={{ color: '#4b5563', lineHeight: '1.6' }}>
              Transcreva o conteúdo extraído do seu PDF correspondente a este módulo aqui.
            </p>
          </section>

        </main>
      </div>
    </div>
  );
}
